<script>
  // Import the data here
  import data from "$data/data.js"; // llamamos data a lo que importamos y el símbolo "$" significa que coja el archivo sin importar dónde se encuentre en la carpeta (en este caso está en una sub carpeta)
  import { scaleLinear } from "d3-scale"; // importamos la librería d3-scale para usar Scalelinear

  const margin = {
    top: 20,
    right: 20,
    bottom: 20,
    left: 0,
  };

  let width = 500;
  let innerWidth = width - margin.left - margin.right;
  let height = 500;
  let innerHeight = height - margin.top - margin.bottom;

  // creamos una escala para el eje x
  let xScale = scaleLinear()
    .domain([0, 100]) // rango de los datos que metems
    .range([0, innerWidth]); // rango en el que queremos que salgan los datos

  // lo mismo para la escala de eje y
  let yScale = scaleLinear().domain([0, 60]).range([innerHeight, 0]); // el eje y va de arriba a abajo

  import AxisX from "$components/AxisX.svelte"; //este es el nombre del archivo y la ruta del archivo. Ahí crearemos el Eje
  //pero necesitamos USARLO dentro de nuestro gráfico, no basta solo con "importarlo" aquí
  import AxisY from "$components/AxisY.svelte";
</script>

<!-- Para que muestre los círculos necesitamos que estén contenidos en un svg y le asignamos la altura y anchura de los ejes-->
<svg {width} {height}>
  <g transform="translate({margin.left}, {margin.top})">
    <!-- hemos creado unos margenes y transformamos el gráfico entero en base a ella creando un grupo -->
    <!--realmente esto equivale a width={width} height={height}, pero como la variable y el valor se llaman igual se puede acortar-->
    <AxisX {xScale} height={innerHeight} width={innerWidth}/>
    <!-- para usar el eje X que hemos creado en el archivo-->
    <!-- xScale={xScale} o {xScale} para importar la función de escala que hemos usado para ubicar los círculos en el nuevo documento para los ejes-->
    <AxisY {yScale} width={innerWidth} />
    <!-- Svelte {#each} block -->
    <!-- creamos un círculo para cada dato-->
    {#each data as d}
      <circle
        cx={xScale(d.grade)}
        cy={yScale(d.hours)}
        r={10}
        fill="steelblue"
        stroke="black"
        stroke-width="1"
      />
    {/each}
  </g>
</svg>

<style>
  /* así estamos seleccionando la clase "tick" que hemos añadido a los grupos de los ejes 
  y con text dentro de la clase .tick el elemento text
  PERO solo se aplica en este archivo. Así que lo envolvemos en :global()*/
  :global(.tick text, .axis-label) { 
    font-weight:400;
    font-size: 12px;
    fill: #000000;
    opacity: 80%;
  }
</style>