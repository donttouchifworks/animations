<template>
<div ref="slots" class="gameItem">
    <div v-if="finalImg" ref="slotItem" class="gameItem__box" :style="`background-image: url(${finalImg})`"></div>
</div>
</template>

<script lang="ts">

import { onMounted, reactive, ref} from "vue";
import mergeImages from 'merge-images';
import {TweenMax} from 'gsap'
interface images {
  src: string,
  x: number,
  y: number,
}
export interface Props {
  config: Array<Number>,
  user: number,
}
export default {
  props:{
    config: Object(),
    user: Number,
  },
  setup(props: Props){
    const slots = ref();
    const pConf = reactive(shuffle(Object.assign([], props.config)))
    const finalImg = ref()
    const images = ref(Array<images>())
    const totalHeight = ref(0)
    const slotItem = ref();
    const imgHeight = 192

    onMounted(() => {
      createImage()
      window.addEventListener('resize', createImage);
    })

    const start = () => {
      const to = detectItem()
      TweenMax.to(slotItem.value, Math.floor(Math.random() * 6) + 1, {
        backgroundPositionY: to.px,
        ease: "Expo.easeInOut"
      }).then(() => {
        console.log(props.user, to.item)
      });
    }

    function detectItem(){
      const indexInConf = Math.floor(Math.random() * props.config.length)
      const item = pConf[indexInConf]
      const times = Math.floor((Math.random() * 9) + 5)
      const pxOftimes = -Number(totalHeight.value * times )
      const correction = 90
      const localPx = Number(-(indexInConf * imgHeight))
      const px= pxOftimes + localPx + correction
      return {px, item}
    }

    // function createWiningIng(id: number){
    //
    // }
    async function createImage() {
      images.value = Array<images>()
      let height = 0;
      pConf.forEach((el: number) => {
        const img = new Image();
        img.src = `/images/${el}.png`;
        images.value.push({src: `/images/${el}.png`, y: Number(height), x: slots.value.offsetWidth/2 - imgHeight/2})
        if(slots.value.offsetHeight < imgHeight){
          height += slots.value.offsetHeight
        } else {
          height += imgHeight
        }

      })

      await mergeImages(images.value, {height: height, width:slots.value.offsetWidth}).then((res) => {finalImg.value = res})
      totalHeight.value = height
    }
    function shuffle (array: number[]) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
      return array;
    };
    return{
      start,
      images,
      slots,
      props,
      pConf,
      finalImg,
      totalHeight,
      slotItem,
      imgHeight
    }
  }
}

</script>

<style scoped lang="scss">
.gameItem{
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  &__box{
    width: 100%;
    height: 100%;
    background-origin: border-box;
    background-repeat-x: no-repeat;
    background-position-y: 90px;
  }
}
img{
}
@-webkit-keyframes MOVE-BG {
  from {
    background-position: 0% 0%
  }
  to {
    background-position: 0% 187%
  }
}
</style>