<template>
  <div class="home">
    <div ref='app'></div>
    <p>{{cookies}}</p>
    <!-- <img alt="Vue logo" src="/images/bg.png"> -->
  </div>
</template>

<script>
import * as PIXI from 'pixi.js'
export default {
  name: 'Home',
  data() {
    return {
      foreground: null,
      background: null,
      dmap: null,
      app: null,
      stage: null,
      size: { w: 480, h: 360 },
      cookies: ''
    }
  },
  created() {
    this.app = new PIXI.Application();

    const cookiesValue = 'KDTSESSIONID=YZ689050770985136128YZjXnJU08Y;yz_log_ftime=1584324107466;yz_log_uuid=6d3292e1-7187-77fc-445e-8b7c508892b4;trace_sdk_context_from_source=;trace_sdk_context_ab_test=;gr_user_id=c9e16877-3411-4473-92f2-7d4452a1a172;grwng_uid=7da6a477-58c0-4f07-8b9b-ff03620b0a22;rdfp=54778af4b820794ab58083be0a00e433;nobody_sign=YZ689050770985136128YZjXnJU08Y;captcha_sid=YZ689874999162630144YZH7uyRAnO;yz_ep_page_type_track=iDJ3GNJDHbhHtOl6W3j3ZA%3D%3D;_canwebp=1;Hm_lvt_7bec91b798a11b6f175b22039b5e7bdd=1584510652,1584520485,1584932106,1585017023;loc_dfp=7f1054812e2c2e4f93f87ca9d6a7eb0b;dfp=f946102fe5a895c6f0e842581015bfb0;Hm_lpvt_7bec91b798a11b6f175b22039b5e7bdd=1585019936;sid=YZ691969420725399552YZJ4kJM9Md;user_id=179569836;mobile=13670548132;user_weixin=%2B86-13670548132;user_nickname=13670548132;kdt_id=45935699;team_auth_key=a76310fd56daad696869985007936fc8;access_token=59ad002a3a5eaabc5a3973394cbf72;_kdt_id_=44291286;UM_distinctid=1711117bc42856-096a3794c6b10a-396c7f07-1fa400-1711117bc439ec;yz_log_seqn=1'
    this.$cookies.config('7d')
    cookiesValue.split(';').forEach((v) => {
      v = v.split('=')
      this.$cookies.set(v[0], v[1])
    })
    this.$cookies.set('KDTSESSIONID', 'YZ687352128645378048YZgtho75nc')
    this.$cookies.set('_kdt_id_', '45935699')
    let t = ''
    this.$cookies.keys().forEach(v => {
      t = t + v+'='+this.$cookies.get(v) + '\n'
    })
    this.cookies = t
    // window.location.href = ('https://cashier.youzan.com/pay/wsctrade_buy?book_key=6644ed01-10f1-4430-846d-d930ecfe00a1')
    window.location.href = ('https://shop42471517.m.youzan.com/wscgoods/detail/278zy4lzuyy85?banner_id=f.84249003~goods.4~1~nOhdB6Ng&components_style_layout=1&reft=1585295247923&spm=f.84249003')
    

    // this.dmap = PIXI.Sprite.fromImage('/images/bg-dmap.png');
  },
  computed: {
  },
  mounted() {
    this.$refs.app.appendChild(this.app.view);
    this.app.renderer.autoResize = true;
    this.app.renderer.resize(window.innerWidth, window.innerHeight);
    this.app.stage.interactive = true
    // this.stage = new PIXI.Container()
    this.foreground = this.createLayer()
    // this.background = this.createLayer();
    //this.background.anchor.set(.5)
    const dmapTexurue = this.createDMap()
    this.dmap = new PIXI.filters.DisplacementFilter(dmapTexurue, 0)
    this.dmap.blendMode = PIXI.BLEND_MODES.LIGHTEN;
    // this.background.filters = [this.dmap]
    this.app.stage.addChild(this.foreground)
    this.app.stage.addChild(dmapTexurue)
    this.foreground.filters = [this.dmap]
    // this.app.stage.addChild(this.background)
    this.app.stage
      .on('pointermove', this.mouseHandle)
    this.app.ticker.add(this.render);
  },
  methods: {
    createLayer() {
      const layer = PIXI.Sprite.from('/images/bg8.png')
      layer.width = this.size.w
      layer.height = this.size.h
      layer.x = document.body.offsetWidth / 2 - layer.width / 2
      layer.y = document.body.offsetHeight / 2 - layer.height / 2
      return layer
    },
    createDMap() {
      const layer = PIXI.Sprite.from('/images/bg8-dmap.png')
      layer.width = this.size.w
      layer.height = this.size.h
      layer.x = document.body.offsetWidth / 2 - layer.width / 2;
      layer.y = document.body.offsetHeight / 2 - layer.height / 2
      return layer
    },
    mouseHandle(e) {
      this.setTilt(40, e.data.global.x, e.data.global.y, this.dmap)
    },
    setTilt(maxTilt, mouseX, mouseY, displacementFilter) {
      var midpointX = document.body.offsetWidth / 2,
          midpointY = document.body.offsetHeight / 2,
          posX = midpointX-mouseX,
          posY = midpointY-mouseY,
          valX = (posX / midpointX) * maxTilt,
          valY = (posY / midpointY) * maxTilt;
      displacementFilter.scale.x = valX / 2;
      displacementFilter.scale.y = valY / 2;
    },
    render() {
    }
  }
}
</script>
<style>
body{
  padding: 0;
  margin: 0;
}
</style>