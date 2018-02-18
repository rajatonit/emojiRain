<template>
<canvas id="canvas"></canvas>
</template>


<style>
#canvas {
  background: transparent;
  position: fixed;
  z-index: -1;
}
</style>


<script>
var active = false;
export default {
  name: 'turbanHeartRain',
  data() {
    return {
      drops: 500,
      dropsForDrawing: [],
      _totalemoji: 0,
      timeout: 0,
      animationFrame : {},
      active: false
    }

  },
  methods: {
    _start() {
      this._resizeWindow();
      this._scaleCanvas();
      //this.active = true;
      this._animate();

      //this.fire('emoji-rain-start');
    },
    _stop() {
      //this.active = false;
      clearTimeout(this.timeout);
        //console.log(this.animationFrame)

        window.cancelAnimationFrame(this.animationFrame);


      this.context.clearRect(0, 0, this.canvas.width, this.canvas.height);
      this.emoji = []
    },
    _animate() {
      var self = this;

      function lol() {

        return setTimeout(function() {

          if(self.active){
          this.animationFrame = window.requestAnimationFrame(lol);
        }else{
          window.cancelAnimationFrame(this.animationFrame);
          clearTimeout(this.timeout);
          clearTimeout(lol());
           this.context.clearRect(0, 0, self.canvas.width, self.canvas.height);
          self.emoji = []


        }
          self.context.clearRect(0, 0, self.canvas.width, self.canvas.height);
          for (var i = 0; i < self.dropsForDrawing.length; i++) {

            self._paintEmoji(self.dropsForDrawing[i]);

          }
        }, 1000 / 60);

      }
      this.timeout = () =>{
        return setTimeout(function() {
          self.animationFrame = window.requestAnimationFrame( lol);
          self.context.clearRect(0, 0, self.canvas.width, self.canvas.height);
          for (var i = 0; i < self.dropsForDrawing.length; i++) {
            self._paintEmoji(self.dropsForDrawing[i]);
          }
        }, 1000 / 60);
      }

      if (this.active) {
        this.timeout(this.active)
      }
    },
    _paintEmoji(emoji) {
      if (emoji.y >= this.canvas.height || emoji.opacity < 0.1) {
        var i = emoji.arrayIndex;
        emoji = this.giveMeARandomEmoji();
        emoji.arrayIndex = i;
        this.dropsForDrawing[i] = emoji;
      } else {
        emoji.y += emoji.speed;
        emoji.opacity -= emoji.opacitySpeed;
      }
      this.context.globalAlpha = emoji.opacity;
      var isEven = emoji.arrayIndex % 2;

      //console.log("hereeweeee")
      this.context.font = isEven ? '20px serif' : '30px serif';
      this.context.fillStyle = 'red';

      this.context.fillText(emoji.char, emoji.x, emoji.y);

      this.context.restore();
    },

    _generateDrops() {
      this.dropsForDrawing = [];
      for (var i = 0; i < this.drops; i++) {
        var emoji = this.giveMeARandomEmoji();
        emoji.arrayIndex = i;
        this.dropsForDrawing.push(emoji);
      }
    },

    _resizeWindow() {
      this.canvas.width = window.innerWidth;
      this.canvas.height = window.innerHeight;
    },

    _scaleCanvas() {
      // Finally query the various pixel ratios.
      var devicePixelRatio = window.devicePixelRatio || 1;
      var backingStoreRatio = this.context.webkitBackingStorePixelRatio ||
        this.context.mozBackingStorePixelRatio ||
        this.context.msBackingStorePixelRatio ||
        this.context.oBackingStorePixelRatio ||
        this.context.backingStorePixelRatio || 1;
      var ratio = devicePixelRatio / backingStoreRatio;
      // Upscale the canvas if the two ratios don't match.
      if (devicePixelRatio !== backingStoreRatio) {
        var oldWidth = this.canvas.width;
        var oldHeight = this.canvas.height;
        this.canvas.width = oldWidth * ratio;
        this.canvas.height = oldHeight * ratio;
        this.canvas.style.width = oldWidth + 'px';
        this.canvas.style.height = oldHeight + 'px';
        // Now scale the context to counter the fact that we've manually scaled
        // our canvas element.
        this.context.scale(ratio, ratio);
      }
    },
    giveMeARandomEmoji: function() {
      var emoji = {}
      //console.log("here")
      emoji.code = this._emoji[Math.floor((Math.random() * this._totalEmoji))];
      emoji.char = this._fromCodePoint(emoji.code);
      //https://vu-js-learning-octate.c9users.io/
      // console.log(emoji.code)
      // 1 to window size
      emoji.x = Math.floor((Math.random() * this.canvas.width) + 1);
      emoji.y = Math.floor((Math.random() * this.canvas.height) + 1);

      emoji.speed = Math.floor(Math.random() * 10 + 1);
      emoji.opacity = 1;
      emoji.opacitySpeed = 0.02 * (Math.random() * 2 + 1);
      return emoji;
    },
    _fromCodePoint: function(codepoint) {
      var code = typeof codepoint === 'string' ?
        parseInt(codepoint, 16) : codepoint;
      if (code < 0x10000) {
        return String.fromCharCode(code);
      }
      code -= 0x10000;
      return String.fromCharCode(
        0xD800 + (code >> 10),
        0xDC00 + (code & 0x3FF)
      )
    }

  },

  mounted() {
    this._emoji = ['1F473', '2764'];
    this._totalEmoji = this._emoji.length;

  //  console.log(this._emoji)
    this._imageTransmogrifier = document.createElement('div');
    this.canvas = document.getElementById('canvas');
    this.context = this.canvas.getContext('2d');
    this.context.fillStyle = 'black';

    this._resizeWindow();
    this._scaleCanvas();

    // Care about window resizing.
    var self = this;
    window.addEventListener('resize', function() {
      self._resizeWindow();
    }, false);

  },
  props: ['rainAway'],
  watch: {
    rainAway: function(val) {

      if (val === false) {
        this.active = false;
      //  console.log(val)
        this._stop();
      } else {
        this.active = true;

        this._generateDrops();
      //  console.log(true)
        this._start();
      }
    }
  }
}
</script>
