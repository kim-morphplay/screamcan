<script>
    import P5Canvas from "./P5Canvas.svelte";
    import { can, texture } from './stores/store.js';
    import { wglcanvas } from './layers/layers.js';

    let canSketch = (p5) => {
        let cnv;
        let rotation = 0;
        const rotationSpeed = 0.006;
        let rotating = true;
        const pauseRotation = () => {
            rotating = false;
        }
        const resumeRotation = () => {
            rotating = true;
        }
        p5.setup = () => {
            p5.pixelDensity(1);
            cnv = wglcanvas(p5, 1.5);
            cnv.mouseOver(pauseRotation);
            cnv.mouseOut(resumeRotation);
        };
        p5.draw = () => {
            p5.background(255, 255, 255);
            //p5.directionalLight(255, 255, 255, 0, 0, -100);
            p5.noStroke();
            if (rotation >= 360) {
                rotation = 0;
            }
            rotation = rotating ? rotation + rotationSpeed : rotation;
            p5.rotateY(-rotation);
            p5.fill(218, 218, 218);
            p5.cylinder(can.radius - 0.9, can.height + 20, 50, 1, true, true);
            p5.texture($texture);
            p5.cylinder(can.radius, can.height, 50, 1, false, false);
        };
    };
</script>

<style>
    #canRender {
        width: 100%;
    }
</style>

<div id="canRender">
    <P5Canvas sketch={canSketch} id="canStage" />
</div>