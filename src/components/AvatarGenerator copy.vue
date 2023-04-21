<script setup lang="ts">
import { ref } from 'vue';

function getRandomElement(array: any[]): number {
    return Math.floor(Math.random() * array.length);
}

function randomInt(min: number, max: number): number { // Return number between min and max
  return Math.floor(Math.random() * (max - min + 1) + min);
}

function renderAvatar(): void {
    const imagesPath: string = "./src/assets/avatar/";
    const imagesArray: any = { // Count of images of this type and offset
        head:       {count: 1,  offset: {x: 0, y: 125}},
        top:        {count: 11, offset: {x: 0, y: 58}},
        mouth:      {count: 7,  offset: {x: 0, y: 225}},
        glasses:    {count: 3,  offset: {x: 0, y: 194}},
        eyes:       {count: 5,  offset: {x: 0, y: 200}},
        eyebrows:   {count: 6,  offset: {x: 0, y: 176}},
        body:       {count: 6,  offset: {x: 0, y: 297}},
        background: {count: 3,  offset: {x: 0, y: 0}},
        pet:        {count: 4,  offset: {x: 300, y: 295, rotation: -20}},
    };
    const imagesTypesOrder: string[]  = [ // Order of rendering types
        "background",
        "head",
        "body",
        "eyes",
        "eyebrows",
        "mouth",
        "glasses",
        "top",
        "pet"
    ];
    

    let allImages: any = []; // All choicen images
    let loadImagesCount: number = 0; // Number of loaded images

    // Choising one image in each category
    for (let type of imagesTypesOrder) {        
        let image = new Image();
        let randomImage = randomInt(1, imagesArray[type].count); // Choice random image of type
        image.src = `${imagesPath}${type}/${randomImage}.svg`; // fullpath + image type + choicen image + .svg
        allImages.push({image: image, offset: imagesArray[type].offset}); // Add choicen image and its type (head / top / etc.)
        
        loadImagesCount++; // Increase all images count
        image.onload = () => loadImagesCount--; // when its loaded - decrease
    };
    
    // Render
    const canvas: any =  document.querySelector(".generator");
    const ctx: any = canvas.getContext("2d");
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    const intervalId = setInterval(() => { 
        if (loadImagesCount === 0) { // When all images is ready - render
            clearInterval(intervalId); // Stop interval
            
            for (let i in allImages) {
                let imageCenterX = (canvas.width - allImages[i].image.width) / 2; // Horizontal center of canvas for image
                let imageCenterY = (canvas.height - allImages[i].image.height) / 2; // Vertical center of canvas for image
                if (!allImages[i].offset?.rotation) {
                    ctx.drawImage(
                        allImages[i].image,
                        imageCenterX - allImages[i].offset.x,
                        allImages[i].offset.y
                    );
                } else { // For pet rotate ctx, place and rotate back
                    const RADIANS  = Math.PI / 180;
                    ctx.rotate(RADIANS * allImages[i].offset?.rotation);
                    ctx.drawImage(
                        allImages[i].image,
                        imageCenterX - allImages[i].offset.x,
                        allImages[i].offset.y
                    );
                    ctx.rotate(RADIANS * allImages[i].offset?.rotation * -1);
                }
                    
            }
        }
    }, 10)
}
</script>

<template>
    <div>
        <canvas class="generator" width="440" height="440"></canvas>
        <button @click="renderAvatar">Сгенерировать</button>
    </div>
</template>