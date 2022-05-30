<template>
<div class="pallatte">
  <button @click="clear" id="clear">
  </button>
    <ul>
    <li v-for="item in items" :key="item" :style="{backgroundColor: item}" @click="()=>drawcolor=item">
      {{ item }}
    </li>
  </ul>
</div>
<canvas id="canvas"
v-on:mousemove="mousemove"
v-on:mousedown="mousedown"
v-on:mouseup="mouseup"
v-on:mouseout="mouseleave"
>
</canvas>
</template>
<script>
export default {
  name: 'App',
  data(){
    return{
      drawcolor:"black",
      items: [
        'red',
        'blue',
        'green',
        'yellow'
      ],
      click:false
    }
  },
  mounted(){
    let x;
    let y;
    this.drawcolor="black";
    const canvas = document.getElementById('canvas');
    canvas.width=window.innerWidth
    canvas.height=window.innerHeight
    const ctx =canvas.getContext('2d');
    this.draw(canvas,ctx,x,y);
    }
,
  methods:{
  clear(){
      const canvas = document.getElementById('canvas');
      const ctx =canvas.getContext('2d');
      ctx.fillStyle = "white";
      ctx.clearRect(0,0,canvas.width,canvas.height);
      ctx.fillRect(0,0,canvas.width,canvas.height);
    },
  draw(canvas,ctx,x,y){
      canvas.onmousedown=(e)=>{
        this.click=true;
        x = e.clientX;
        y = e.clientY;
        ctx.moveTo(x,y);
      }
      canvas.onmouseup=(e)=>{
        if(this.click){
         ctx.stroke();
         ctx.closePath();
         this.click=false;
        }
         e.preventDefault();
      }
      canvas.onmouseout=(e)=>{
        if(this.click){
        ctx.stroke();
        ctx.closePath();
        this.click=false;
        }
        e.preventDefault();
      }
      canvas.onmousemove = (e)=>{
        x = e.clientX;
        y = e.clientY; 
        if(this.click){
        ctx.lineTo(x,y);
        ctx.strokeStyle=this.drawcolor;
        ctx.lineCap="round";
        ctx.lineWidth=10;
        ctx.lineJoin="round";
        ctx.stroke();
         e.preventDefault();
      }
      }
  }
  }
}
</script>
<style>
#clear{
  width:100px;
  height:100px;
  top:0;
  left:0;
  background-color:rgb(150, 0, 0);
  cursor:pointer;

}
body{
  margin:0;
  padding:0;
}
#canvas{
  top:0;
  left:0;
  bottom:0;
  right:0;
  margin:0;
  padding:0;
  background-color: rgb(255, 255, 255);
}
.pallatte{
  width:50px;
  position:absolute;
  top:0;
  left:0;
  bottom:0;
  right:0;
  margin:0;
  padding:0;
}
ul{
  cursor: pointer;
  list-style:none;
  padding:0;
  margin:0;
}

</style>
