<template lang="pug">
    #canvas(class="f98")
</template>

<script>
import * as THREE from "three";
import { ColladaLoader } from 'three/examples/jsm/loaders/ColladaLoader';
//import scene from '../../assets/services/scene'

export default {
    name: "HelloWorld",
    data() {
        return {
        background: 0x0080ff,
        alpha: 0,
        canvas: "canvas",
        width: "100%",
        height: window.innerHeight+"px",
        pading:0
        };
    },
    methods: {
        // pinaCollada(modelname, scale,material) { 
        //     let that = this
        //     var loader = new ColladaLoader(); 
        //     var localObject; 
        //     //loader.options.convertUpAxis = true;
        //     loader.setPath("src/assets/models/");
        //     loader.load(modelname+'.dae', function colladaReady(collada) { 
        //     localObject = collada.scene
        //     //localObject.children[0].material = new THREE.MeshBasicMaterial( { color: 0xff0000, wireframe: true } );
        //     localObject.children[0].material = material
        //     console.log("____________________OBJ")
        //     console.log(localObject.children[0])
        //     console.log("____________________OBJ")
            
        //     localObject.position.set(10,10,0)
        //     localObject.scale.x = localObject.scale.y = localObject.scale.z = scale; 
        //     localObject.updateMatrix();
            
            
        //     that.mesh2=localObject
        //     that.scene.add( that.mesh2 );
        //     //return localObject
        //     }); 
        // } ,
        // init: function() {
        //     let container = document.getElementById(this.canvas);
        //     container.style.width = this.width;
        //     container.style.height = this.height;
        //     const light = new THREE.AmbientLight( 0x404040 );
        //     this.renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
        //     this.renderer.setClearColor(this.background, this.alpha);
        //     this.renderer.setSize(container.clientWidth, container.clientHeight);
        //     container.appendChild(this.renderer.domElement);
        //     this.scene = new THREE.Scene();
        //     this.camera = new THREE.OrthographicCamera(
        //         container.clientWidth /-50,
        //         container.clientWidth /50,
        //         container.clientHeight/50,
        //         container.clientHeight/-50,
        //         1,
        //         100
        //     );
        //     this.camera.position.z = 10;
        //     this.scene = new THREE.Scene();
        //     const geometry = new THREE.BoxGeometry(1,1, 1);
        //     const material = new THREE.MeshNormalMaterial();
        //     const material2 = new THREE.MeshNormalMaterial();

        //     this.pinaCollada('bola',1,material2)

        //     this.mesh = new THREE.Mesh(geometry, material);
        //     this.scene.add(this.mesh);
        //     this.scene.add(light)

        //     this.ani();
        // },
        // ani: function() {
        //     requestAnimationFrame(this.ani);
        //     this.mesh.rotation.z += 0.01;
        //     console.log("333")
        //     console.log(this.mesh2)
        //     this.mesh2.rotation.z += 0.01;
        //     this.renderer.render(this.scene, this.camera);
        // },
        
        objProperties:function (obj,material,scale){
            obj.children[0].material = material
            console.log("____________________OBJ")
            console.log(obj.children[0])
            console.log("____________________OBJ")
            
            obj.position.set(10,10,0)
            obj.scale.x = obj.scale.y = obj.scale.z = scale; 
            obj.updateMatrix();
            return obj
        },
        pinaCollada:async function(modelname, scale,material) { 
            let that = this
            
            let obj 
            //loader.options.convertUpAxis = true;
            
            that.loader.load(modelname+'.dae', async function colladaReady(collada) { 
            obj = collada.scene
            //localObject.children[0].material = new THREE.MeshBasicMaterial( { color: 0xff0000, wireframe: true } );
            
            //that.mesh=localObject
            //that.scene.add( that.mesh );
            that.obj = await that.objProperties(obj,material,scale)
            //that.mesh2=localObject
            that.scene.add( that.obj );
            //return localObject
            }); 
        },
        mountedScene:function(){
            let container = document.getElementById(this.canvas);
            container.style.width = this.width;
            container.style.height = this.height;
            const light = new THREE.AmbientLight( 0x404040 );
            this.renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
            this.renderer.setClearColor(this.background, this.alpha);
            this.renderer.setSize(container.clientWidth, container.clientHeight);
            container.appendChild(this.renderer.domElement);
            this.scene = new THREE.Scene();
            this.camera = new THREE.OrthographicCamera(
                container.clientWidth /-50,
                container.clientWidth /50,
                container.clientHeight/50,
                container.clientHeight/-50,
                1,
                100
            );
            this.camera.position.z = 10;
            this.scene = new THREE.Scene();
            this.scene.add(light)
            this.loader = new ColladaLoader(); 
            this.loader.setPath("src/assets/models/");
            // const material2 = new THREE.MeshNormalMaterial();
            // this.pinaCollada('bola',1,material2)
            //this.animation()
        },
        mountedObjs:function(){
            const material2 = new THREE.MeshNormalMaterial();
            this.pinaCollada('bola',1,material2)
            //this.scene.add(this.mesh2)
        },
        animation: function() {
            requestAnimationFrame(this.animation);
            //this.mesh.rotation.z += 0.01;
            console.log("333")
            console.log(this.mesh2)
            try{
                this.obj.rotation.z += 0.01;
            }catch(error){
                console.error("Err Int"+error)
            }
            this.renderer.render(this.scene, this.camera);
        },


    },
    mounted() {
        // this.init();
        // 
        this.mountedScene()
        this.mountedObjs();
        this.animation();
        
        
    },
}
</script>
<style lang="css">
    @import '../.././assets/styles/components.css' 
    
</style>