<template>
  <div>
    <div class="topheader">
      <img class="responsive" style="z-index:99;top:50px;width: 200px" src="/img/hea2-new.png"/>
    </div>
    <div class="vongquay">
      <canvas id="canvas" width="450" height="450" data-responsiveMinWidth="180" data-responsiveScaleHeight="true"
              data-responsiveMargin="50">
      </canvas>
      <div class="startBtn"></div>
      <div @click.prevent="startSpin()" class="btnSpin"></div>
    </div>
    <img class="responsive2" style="position:fixed;z-index:99;top:0px;right:0px" src="/img/hea1.png"/>
    <div
      style="position:fixed;z-index:99;bottom:-50px;left:0;width:100%;height:500px;background:url(/img/bot1.png) repeat-x bottom left;"></div>
  </div>
</template>

<script>
import * as Winwheel from '~/plugins/winwheel'
import Cookies from 'js-cookie'

export default {
  name: 'SpinWheel',
  data() {
    return {
      times: 0,
      theWheel: null,
      wheelSpinning: false,
      segments: [
        {
          'fillStyle': '#ab6f03',
          'text': '10.000',
          'textFontSize': 28,
          'textFillStyle': '#ffffff'
        },
        {
          'fillStyle': '#910f06',
          'text': '20.000 VNĐ',
          'textFontSize': 30,
          'textFillStyle': '#ffffff'
        }, {
          'fillStyle': '#ab6f03',
          'text': '50.000 VNĐ VNĐ',
          'textFontSize': 28,
          'textFillStyle': '#ffffff'
        }, {
          'fillStyle': '#910f06',
          'text': '100.000 VNĐ VNĐ',
          'textFontSize': 26,
          'textFillStyle': '#ffffff'
        }, {
          'fillStyle': '#ab6f03',
          'text': '200.000 VNĐ',
          'textFontSize': 24,
          'textFillStyle': '#ffffff'
        },
        {
          'fillStyle': '#910f06',
          'text': '500.000 VNĐ',
          'textFontSize': 24,
          'textFillStyle': '#ffffff'
        }
      ],
      messages: [
        'Chúc người anh yêu có một năm mới vui vẻ và nhiều niềm vui. Mong rằng mọi dự định của em trong năm mới đều hoàn thành như ý. Anh sẽ luôn bên cạnh và ủng hộ em mọi lúc. Yêu em rất nhiều',
        'Năm qua anh đã rất vui vì có em ở bên. Em không biết sự tồn tại của em có ý nghĩa với anh đến nhường nào đâu. Vậy nên anh mong rằng trên mọi chặng đường mà anh đi đều hiện diện hình bóng của em. Chúc mừng năm mới người yêu của anh.',
        'Anh biết một năm qua đã có nhiều điều khiến em phải suy nghĩ nhiều, khiến em buồn. Nhưng hãy tin tưởng anh nhé. Anh nhất định sẽ làm được. Cám ơn em vì đã ở bên anh. Chúc em năm mới vui vẻ.',
        'Chúc em năm mới với những điều tích cực mới sẽ đến với em. Hãy luôn cố gắng suy nghĩ tích cực và lạc quan em nhé. Yêu em rất nhiều!',
      ]
    }
  },
  methods: {
    resetWheel() {
      this.$swal({
        title: '',
        text: 'Năm mới vui vẻ và tích cực em nha. Chơi game có thưởng nè hị hị. Xiền thật đó nha. Em có 3 lượt quay nha 😜. Chúc em yêu luôn vui vẻ, yêu em nhiều',
        confirmButtonText: 'Bắt đầu',
        confirmButtonColor: '#AC021B'
      });
      this.theWheel = new Winwheel({
        'outerRadius': 200, // Bán kính ngoài
        'innerRadius': 0, // Size lỗ trung tâm
        'textFontSize': 24, // Size chữ
        'textOrientation': 'horizontal', // Chữ nằm ngang
        'textAlignment': 'outer', // Căn chỉnh văn bản ra bên ngoài bánh xe.
        'numSegments': this.segments.length, // Số ô
        'segments': this.segments,
        'animation': // Chỉ định hình động để sử dụng.
          {
            // 'type': 'spinToStop',
            // 'duration': 20, // Thời lượng tính bằng giây.
            'spins': 10, // Số vòng quay hoàn chỉnh mặc định.
            // 'callbackFinished': () => {console.log('3123123')},
            'callbackFinished': this.alertPrize,
            'soundTrigger': 'pin', // Chỉ định các chân là để kích hoạt âm thanh, tùy chọn khác là 'phân đoạn'.
            'type': 'spinToStop',
            'duration': 6.4,
          },
        'pins': {
          'number': this.segments.length, // Số lượng chân. Họ không gian đều xung quanh bánh xe.
          'responsive': true,
          'fillStyle': 'silver',
          'outerRadius': 4,
        }
      })

      if (this.wheelSpinning) {
        this.theWheel.stopAnimation(false) // Stop the animation, false as param so does not call callback function.
      }
      this.theWheel.rotationAngle = 0 // Re-set the wheel angle to 0 degrees.
      this.theWheel.draw() // Call draw to render changes to the wheel.
      this.wheelSpinning = false // Reset to false to power buttons and spin can be clicked again.
    },
    initSpin() {
      let times = Cookies.get('times')
      if (times === undefined) {
        times = 3
        Cookies.set('times', times)
      }
      this.times = times
      this.resetWheel()
    },
    startSpin() {
      if (this.wheelSpinning === false) {
        if (this.times < 1) {
          this.$swal({
            title: 'Em chỉ có 3 lượt quay thui ạ. Anh hết xiền rồi 😓',
            showConfirmButton: false,
            timer: 2000,
          });
          return
        }
        this.theWheel.startAnimation()
        this.wheelSpinning = true
      }
    },
    alertPrize(indicatedSegment) {
      const prize = indicatedSegment.text
      this.theWheel.rotationAngle = 0; // Đặt lại góc bánh xe về 0 độ.
      this.theWheel.draw(); // Gọi draw để hiển thị các thay đổi cho bánh xe.
      this.wheelSpinning = false; // Đặt lại thành false thành các nút nguồn và quay có thể được bấm lại.
      const random = Math.floor(Math.random() * this.messages.length);
      this.$swal({
        title: `Chúc mừng em nhận được ${prize}`,
        text: this.messages[random],
        confirmButtonText: 'Tiếp tục',
        confirmButtonColor: '#AC021B'
      });
      if (this.times > 0) {
        this.times--;
        Cookies.set('times', this.times)
      }
      $nuxt.$axios.post('/api/send', {
        mess: `Thông báo giải thưởng: ${prize}`
      })
    }
  },
  mounted() {
    this.initSpin()
  },
}
</script>

<style>
body {
  background: url("/img/background.jpg") no-repeat fixed center;
  background-position: center center;
  background-size: cover;
  font-size: 30px;
  color: white;
}

.startBtn {
  background-image: url('/img/contro.png');
  position: absolute;
  width: 180px;
  height: 180px;
  z-index: 99;
}

.btnSpin {
  width: 70px;
  height: 70px;
  position: absolute;
  cursor: pointer;
  z-index: 999;
}

.vongquay,
.topheader {
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

@media only screen and (max-width: 400px) {
  .responsive {
    width: 200px;
    height: 150px;
  }

  .responsive2 {
    width: 200px;
    height: 200px;
  }
}

@media only screen and (max-width: 700px) {
  .responsive3 {
    width: 200px;
    height: 162px;
  }
}
</style>
