<template>
  <div id="threepipe-canvas-container">
    <canvas id="threepipe-canvas"></canvas>
  </div>
</template>

<script>
export default {
  name: 'IphoneModel',
  data() {
    return {
      viewer: null,
      state: {
        focused: '',
        hover: '',
        animating: false,
      },
      nextState: {
        focused: '',
        hover: '',
      }
    }
  },
  async mounted() {
    await this.init()
  },
  beforeUnmount() {
    // 清理资源
    if (this.viewer) {
      this.viewer.dispose()
    }
  },
  methods: {
    async init() {
      // 动态导入threepipe
      const {
        CameraViewPlugin, CanvasSnapshotPlugin,
        ContactShadowGroundPlugin,
        PhysicalMaterial,
        PickingPlugin,
        PopmotionPlugin, SRGBColorSpace,
        ThreeViewer,
        timeout,
        TransformAnimationPlugin,
        TransformControlsPlugin,
        CameraView,
        Vector3,
      } = await import('https://unpkg.com/threepipe@0.0.33/dist/index.mjs')

      this.viewer = new ThreeViewer({
        canvas: document.getElementById('threepipe-canvas'),
        msaa: false,
        renderScale: 'auto',
        dropzone: {
          allowedExtensions: ['png', 'jpeg', 'jpg', 'webp', 'svg', 'hdr', 'exr'],
          autoImport: true,
          addOptions: {
            disposeSceneObjects: false,
            autoSetBackground: false,
            autoSetEnvironment: true,
          },
        },
        plugins: [PickingPlugin, PopmotionPlugin,
          CameraViewPlugin, TransformAnimationPlugin,
          new TransformControlsPlugin(false),
          CanvasSnapshotPlugin,
          ContactShadowGroundPlugin],
      })

      const devices = await this.viewer.load('/models/tabletop_macbook_iphone.glb')
      if (!devices) return

      const macbook = devices.getObjectByName('macbook')
      if (macbook) macbook.parent?.remove(macbook)
      const iphone = devices.getObjectByName('iphone')
      if (!iphone) return

      const macbookScreen = macbook?.getObjectByName('Bevels_2')
      if (macbookScreen) macbookScreen.name = 'Macbook Screen'

      this.viewer.assetManager.addEventListener('loadAsset', (e)=>{
        if (!e.data?.isTexture) return
        const texture = e.data
        texture.colorSpace = SRGBColorSpace
        const iPhoneScreen = this.viewer.scene.getObjectByName('xXDHkMplTIDAXLN')?.material
        if(!(iPhoneScreen instanceof PhysicalMaterial)) return
        iPhoneScreen.emissiveMap = texture
        iPhoneScreen.setDirty()
      })

      const isMobile = ()=>window.matchMedia('(max-width: 768px)').matches
      const viewName = (key) => isMobile() ? key + '2' : key

      const transformAnim = this.viewer.getPlugin(TransformAnimationPlugin)
      const cameraView = this.viewer.getPlugin(CameraViewPlugin)

      const picking = this.viewer.getPlugin(PickingPlugin)
      picking.widgetEnabled = false
      picking.hoverEnabled = true

      if (macbookScreen) await transformAnim.animateTransform(macbookScreen, 'closed', 50)?.promise
      await transformAnim.animateTransform(iphone, 'facedown', 50)?.promise
      await cameraView.animateToView(viewName('iphone'), 50)
      const currentView = cameraView.getView()
      const desiredDir = new Vector3(1, 0, 0).normalize()
      const currentDistance = currentView.position.clone().sub(currentView.target).length()
      const obliquePosition = currentView.target.clone().add(desiredDir.multiplyScalar(currentDistance))
      await cameraView.animateToView(new CameraView('oblique', obliquePosition, currentView.target), 200)
      await this.viewer.fitToView(iphone, 1.15, 300)

      // 保存引用以便在事件处理中使用
      this.macbook = macbook
      this.iphone = iphone
      this.macbookScreen = macbookScreen
      this.transformAnim = transformAnim
      this.cameraView = cameraView
      this.isMobile = isMobile
      this.viewName = viewName

      const deviceFromHitObject = (object) => {
        let device = ''
        object.traverseAncestors(o => {
          if (o === this.macbook) device = 'macbook'
          if (o === this.iphone) device = 'iphone'
        })
        return device
      }

      picking.addEventListener('hoverObjectChanged', async(e) => {
        const object = e.object
        if (!object) {
          if (this.state.hover && !this.state.focused) await this.setState({hover: '', focused: ''})
          return
        }
        if (this.state.focused) return
        const device = deviceFromHitObject(object)
        await this.setState({hover: device, focused: ''})
      })

      picking.addEventListener('hitObject', async(e) => {
        const object = e.intersects.selectedObject
        if (!object) {
          if (this.state.focused) await this.setState({hover: '', focused: ''})
          return
        }
        const device = deviceFromHitObject(object)
        e.intersects.selectedObject = device === 'macbook' ? this.macbook : this.iphone
        await this.setState({focused: device, hover: ''})
      })

      document.addEventListener('keydown', (ev)=>{
        if (ev.key === 'Escape' && this.state.focused) this.setState({hover: '', focused: ''})
      })
    },
    async updateState() {
      if (this.state.animating) return
      const next = this.nextState
      if (next.focused === this.state.focused && next.hover === this.state.hover) return
      this.state.animating = true
      const isOpen = this.state.focused
      Object.assign(this.state, next)
      
      if (!this.viewer || !this.iphone) {
        this.state.animating = false
        return
      }
      
      if (this.state.focused) {
        await Promise.all([
          this.macbookScreen ? this.transformAnim.animateTransform(this.macbookScreen, this.state.focused === 'macbook' ? 'open' : 'closed', 500)?.promise : null,
          this.transformAnim.animateTransform(this.iphone, this.state.focused === 'iphone' ? 'floating' : 'facedown', 500)?.promise,
          this.cameraView.animateToView(this.viewName('iphone'), 500),
        ])
        await this.viewer.fitToView(this.iphone, 1.15, 300)
      } else if (this.state.hover) {
        await Promise.all([
          this.macbookScreen ? this.transformAnim.animateTransform(this.macbookScreen, this.state.hover === 'macbook' ? 'hover' : 'closed', 250)?.promise : null,
          this.transformAnim.animateTransform(this.iphone, this.state.hover === 'iphone' ? 'tilted' : 'facedown', 250)?.promise,
        ])
      } else {
        const duration = isOpen ? 500 : 250
        await Promise.all([
          this.macbookScreen ? this.transformAnim.animateTransform(this.macbookScreen, 'closed', duration)?.promise : null,
          this.transformAnim.animateTransform(this.iphone, 'facedown', duration)?.promise,
          isOpen ? this.cameraView.animateToView(this.viewName('iphone'), duration) : null,
        ])
        await this.viewer.fitToView(this.iphone, 1.15, 300)
      }
      this.state.animating = false
    },
    async setState(next) {
      Object.assign(this.nextState, next)
      while (this.state.animating) {
        await new Promise(resolve => setTimeout(resolve, 50))
      }
      await this.updateState()
    }
  }
}
</script>

<style scoped>
:root {
  font-family: Inter, system-ui, Avenir, Helvetica, Arial, sans-serif;

  color-scheme: light dark;
  color: rgba(255, 255, 255, 0.87);
  background-color: #242424;

  font-synthesis: none;
  text-rendering: optimizeLegibility;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
@media (prefers-color-scheme: light) {
  :root {
    color: #213547;
    background-color: #ffffff;
  }
}

body {
  overflow: hidden;
  margin: 0;
}

#threepipe-canvas {
  width: 100%;
  height: 100%;
}

#threepipe-canvas-container{
  position: relative;
  width: 100%;
  height: 100%;
  margin: 0;
  border-radius: 0.5rem;
  box-shadow: rgba(0, 0, 0, 0.25) 0px 25px 50px -12px;
}
</style>
