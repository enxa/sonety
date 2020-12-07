<style>
  svg {
    width: 50vw;
    height: 50vh;
    animation: animateStrokeShadow 5s ease infinite alternate-reverse forwards;
  }
    svg path {
      stroke: #fff;
      stroke-linecap: round;
      stroke-width: .5px;
      fill: none;
      animation: animateStroke 5s ease infinite alternate-reverse forwards;
    }

  @keyframes animateStroke {
    0% { stroke-dasharray: 321px 321px; stroke-dashoffset: 321px; }
    100% { stroke-dasharray: 321px 321px; stroke-dashoffset: 0px; }
  }

  @keyframes animateStrokeShadow {
    0% { filter: drop-shadow( 0 0 10px rgb(255, 255, 255));  }
    100% { filter: drop-shadow( 0 0 10px rgb(0, 255, 255));  }
  }
</style>

<svg width="100%" height="100%" viewBox="0 0 75 70">
  <path d="M19.413,6.084c1.022,-0.588 2.071,-1.129 3.144,-1.616c1.892,-0.859 3.859,-1.554 5.87,-2.075c2.913,-0.754 5.918,-1.143 8.927,-1.155c3.007,-0.012 6.015,0.352 8.933,1.083c2.063,0.516 4.08,1.215 6.019,2.086c1.122,0.503 2.216,1.064 3.281,1.677l-16.268,28.2c0.256,0.151 0.495,0.331 0.707,0.539c0.45,0.439 0.782,0.994 0.954,1.598c0.092,0.322 0.136,0.656 0.137,0.99l32.556,0c0,12.924 -6.894,24.865 -18.086,31.327l-16.278,-28.194c-0.533,0.308 -1.15,0.484 -1.809,0.484c-0.659,0 -1.276,-0.176 -1.809,-0.484l-16.278,28.194c-11.192,-6.462 -18.086,-18.403 -18.086,-31.327l32.556,0c0,-1.338 0.728,-2.507 1.808,-3.133l-16.278,-28.194Z"/>
</svg>