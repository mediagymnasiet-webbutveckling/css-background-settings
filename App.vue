<template>
<div id="app">
    <nav class="side-bar">
        <button @click="reset">Reset</button>
        <label>border</label>
        <input type="range" min="0" max="30" v-model="border">
        <label>padding: {{padding}}px</label>
        <input type="range" min="0" max="20" v-model="padding">
        <label>width: {{width}}px</label>
        <input type="range" min="0" max="1000" v-model="width">
        <label>height: {{height}}px</label>
        <input type="range" min="0" max="1000" v-model="height">
        <label>background-repeat</label>
        <select v-model="backgroundRepeat">
            <option value="no-repeat" selected>no-repeat</option>
            <option value="repeat-x">repeat-x</option>
            <option value="repeat">repeat</option>
            <option value="space">space</option>
            <option value="round">round</option>
        </select>
        <label>background-position</label>
        <span>
            <select v-model="backgroundPosition">
                <option value="center" selected>center</option>
                <option value="top">top</option>
                <option value="right">right</option>
            </select>
            <select v-model="backgroundPosition2">
                <option value="none" selected>none</option>
                <option value="center">center</option>
                <option value="top">top</option>
                <option value="right">right</option>
            </select>

        </span>
        <label>background-attachment</label>
        <select v-model="backgroundAttachment">
            <option value="initial">initial</option>
            <option value="fixed">fixed</option>
            <option value="scroll">scroll</option>
        </select>
        <label>background-image</label>
        <select v-model="currentImage">
            <option v-for="image in images" :value="image" :key="image">{{image}}</option>
        </select>
        <label>background-size</label>
        <span>
            <select v-model="backgroundSize">
                <option value="initial" selected>initial</option>
                <option value="contain">contain</option>
                <option value="cover">cover</option>
                <option value="auto">auto</option>
                <option value="percentage">percentage</option>
            </select>
            <input type="range" min="0" max="200" v-model="backgroundSizePercantage" v-if="backgroundSize === 'percentage'">
        </span>
        <label>background-clip</label>
        <select v-model="backgroundClip">
            <option value="border-box">border-box</option>
            <option value="padding-box">padding-box</option>
            <option value="content-box">content-box</option>
            <option value="text">text</option>
        </select>
        <code>
            <pre>
{{styleFormatted}}
            </pre>
        </code>
    </nav>
    <main>
        <div class="img" :style="style">
            <p>{{currentImage}}</p>
        </div>
    </main>
</div>
</template>

<script>
export default {
    data: () => ({
        padding: 0,
        width: 640,
        height: 640,
        border: 1,
        currentImage: 'tree.jpg',
        backgroundRepeat: 'no-repeat',
        backgroundPosition: 'center',
        backgroundPosition2: '',
        backgroundAttachment: 'initial',
        backgroundImage: 'tree.jpg',
        backgroundSize: 'initial',
        backgroundSizePercantage: 100,
        backgroundClip: 'border-box',
        images: ['tree.jpg', 'cat.jpg'],
        defaultConfig: null
    }),

    mounted(){
        let defaultConfig = this.$options.data();
        this.defaultConfig = defaultConfig;
    },

    computed: {
        code(){
            return "myCode"
        },
        
        styleFormatted(){
            let style = this.style
            return ".img {\n" + Object.keys(style)
            .filter(property => style[property] !== null)
            .map( property => `\t${property}: ${style[property]}`)
            .join("\n")
            + "\n}"
        },

        style(){
            return {
                'padding': `${this.padding}px`,
                'border': `${this.border}px dashed black`,
                'width': `${this.width}px`,
                'height': `${this.height}px`,
                'background-repeat' : this.backgroundRepeat,
                'background-image': `url(${this.currentImage})`,
                'background-position': this.backgroundPosition + (this.backgroundPosition2!="none"?" "+this.backgroundPosition2:""),
                'background-attachment': this.backgroundAttachment,
                'background-size': this.backgroundSize === 'percentage' ? `${this.backgroundSizePercantage}%` : this.backgroundSize,
                'background-clip': this.backgroundClip,
                '-webkit-background-clip': this.backgroundClip == 'text' ? 'text' : null,
                'color': this.backgroundClip == 'text' ? 'transparent' : null,
            }
        }
    },

    methods: {
        reset(){
            for(let key in this.$options.data()){
                if(key === 'defaultConfig' || key === 'images') continue;
                this[key] = this.defaultConfig[key]
            }
        }
    }
}
</script>

<style>
body{
    margin: 0;
    height: 100vh;
    font-family: 'Arial'
}

#app{
    display: grid;
    grid-template-areas: "sidebar main";
    grid-template-columns: 350px auto;
    background-color: white;  
    height: 100%;  
}

nav.side-bar{
    border-top-right-radius: 0.25rem;
    border-bottom-right-radius: 0.25rem;
    box-shadow: 3px 0px 5px 0px rgba(250,195,250,1);
    background: rgb(198,139,203);
    background: linear-gradient(310deg, rgba(198,139,203,1) 1%, rgba(49,0,255,1) 100%);
    display: grid;
    grid-template-columns: 50% 50%;
    grid-template-rows: 3rem repeat(10,3rem);
    align-items: center;
    color: white;
    font-size: 0.9rem;
}

nav.side-bar > input,
nav.side-bar > select{
    margin: auto 1rem auto 1rem;    
}
nav.side-bar > span > select:first-of-type{
    margin: auto 0 auto 1rem;    
}
nav.side-bar > label{
    justify-self: end;
    margin-right: 0.5rem;
}

nav.side-bar > button:first-of-type {
    display: block;
    height: 100%;
    grid-column: span 2;
    outline: none;
    appearance: none;
    font-size: 2rem;
    padding: 0;
    text-transform: uppercase;
    background-color: rgba(0,0,0,0.25);
    border: 0;
    color: white;
    transition: background-color 0.25s;
    cursor: pointer;
}
nav.side-bar > button:first-of-type:hover {
    background-color: rgba(0,0,0,0.1)
}
nav.side-bar > button:first-of-type:active {
    background-color: rgba(0,0,0,0.6)
}

main{
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
}

.img{
    border: 1px solid black;
    transition: background-position 0.25s;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 2rem;
    color: rgba(255,255,255,1);
}
code{
    background-color: rgba(0,0,0,0.5);
    grid-column: span 2;
}
</style>