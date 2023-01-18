<template>
  <div>
    <div class="topheader">
      <img class="responsive" style="z-index:99;top:50px;" src="/img/hea2.png" />
    </div>
    <div class="vongquay">
      <canvas id="canvas" width="450" height="450" data-responsiveMinWidth="180" data-responsiveScaleHeight="true"
        data-responsiveMargin="50">
      </canvas>
      <div class="startBtn"></div>
      <div @click.prevent="startSpin()" class="btnSpin"></div>
    </div>
  </div>
</template>

<script>
import * as Winwheel from 'Winwheel'
// import winwheelPercentToDegrees from 'Winwheel'

export default {
  name: 'SpinWheel',
  data() {
    return {
      theWheel: null,
      wheelSpinning: false,
      segments: [{
          'fillStyle': '#910f06',
          'text': 'Ô mất lượt',
          // 'size': winwheelPercentToDegrees(24),
          'textFontSize': 30,
          'textFillStyle': '#ffffff'
        }, {
          'fillStyle': '#ab6f03',
          'text': '1.000 VNĐ',
          // 'size': winwheelPercentToDegrees(20),
          'textFontSize': 28,
          'textFillStyle': '#ffffff'
        }, {
          'fillStyle': '#910f06',
          'text': '2.000 VNĐ',
          // 'size': winwheelPercentToDegrees(15),
          'textFontSize': 26,
          'textFillStyle': '#ffffff'
        }, {
          'fillStyle': '#ab6f03',
          'text': '5.000 VNĐ',
          // 'size': winwheelPercentToDegrees(15),
          'textFontSize': 24,
          'textFillStyle': '#ffffff'
        }, {
          'fillStyle': '#910f06',
          'text': '10.000 VNĐ',
          // 'size': winwheelPercentToDegrees(12),
          'textFontSize': 22,
          'textFillStyle': '#ffffff'
        }, {
          'fillStyle': '#ab6f03',
          'text': '20.000 VNĐ',
          // 'size': winwheelPercentToDegrees(10),
          'textFontSize': 20,
          'textFillStyle': '#ffffff'
        }, {
          'fillStyle': '#910f06',
          'text': '50.000 VNĐ',
          // 'size': winwheelPercentToDegrees(3),
          'textFontSize': 18,
          'textFillStyle': '#ffffff'
        }, {
          'fillStyle': '#ab6f03',
          'text': '100.000 VNĐ',
          // 'size': winwheelPercentToDegrees(3),
          'textFontSize': 16,
          'textFillStyle': '#ffffff'
        }]
    }
  },
  methods: {
    resetWheel() {
      this.theWheel = new Winwheel({
        'outerRadius': 220, // Bán kính ngoài
        'innerRadius': 0, // Size lỗ trung tâm
        'textFontSize': 24, // Size chữ
        'textOrientation': 'horizontal', // Chữ nằm ngang
        'textAlignment': 'outer', // Căn chỉnh văn bản ra bên ngoài bánh xe.
        'numSegments': this.segments.length, // Số ô
        'segments': this.segments,
        'animation': // Chỉ định hình động để sử dụng.
        {
          'type': 'spinToStop',
          'duration': 20, // Thời lượng tính bằng giây.
          'spins': 10, // Số vòng quay hoàn chỉnh mặc định.
          'callbackFinished': this.alertPrize,
          'callbackSound': this.playSound, // Chức năng gọi khi âm thanh đánh dấu được kích hoạt.
          'soundTrigger': 'pin', // Chỉ định các chân là để kích hoạt âm thanh, tùy chọn khác là 'phân đoạn'.
          'type': 'spinToStop',
          'duration': 6.4,
        },
        'pins': {
          'number': 8, // Số lượng chân. Họ không gian đều xung quanh bánh xe.
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
      this.resetWheel()
    },
    startSpin() {
      if (this.wheelSpinning === false) {
        this.theWheel.startAnimation()
        this.wheelSpinning = true
      }
    },
    alertPrize(indicatedSegment) {
      console.log('123123');
      console.log(indicatedSegment, this.theWheel);
      this.theWheel.rotationAngle = 0; // Đặt lại góc bánh xe về 0 độ.
      this.theWheel.draw(); // Gọi draw để hiển thị các thay đổi cho bánh xe.
      this.wheelSpinning = false; // Đặt lại thành false thành các nút nguồn và quay có thể được bấm lại.
    },
    playSound() {
      console.log('fasdasd');
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
</style>
