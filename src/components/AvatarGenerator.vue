<script setup lang="ts">
import AvatarDownloader from "./AvatarDownloader.vue"
import UButton from './UButton.vue';

function getRandomElement(array: any[]): number {
    return Math.floor(Math.random() * array.length);
}

function randomInt(min: number, max: number): number { // Return number between min and max
  return Math.floor(Math.random() * (max - min + 1) + min);
}

function renderAvatar(): void {
    const imagesPath: string = "./avatar/";
    const imagesArray: any = { // Count of images of this type and offset
        head:       {count: 1,  offset: {x: 0, y: 125}},
        top:        {count: 11, offset: {x: 0, y: 58}},
        mouth:      {count: 7,  offset: {x: 0, y: 225, alt_y: 171}},
        glasses:    {count: 3,  offset: {x: 0, y: 194}},
        eyes:       {count: 5,  offset: {x: 0, y: 200}},
        eyebrows:   {count: 6,  offset: {x: 0, y: 176}},
        body:       {count: 6,  offset: {x: 0, y: 297, alt_y: 290}},
        background: {count: 3,  offset: {x: 0, y: 0}},
        pet:        {count: 4,  offset: {x: 280, y: 125, alt_x: 290, alt_y: 170, rotation: -20}},
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
        allImages.push({image: image, offset: imagesArray[type].offset, type: type}); // Add choicen image and its offset
        
        loadImagesCount++; // Increase all images count
        image.onload = () => loadImagesCount--; // when its loaded - decrease
    };
    
    // Render
    const canvas: any =  document.querySelector(".generator__canvas");
    const ctx: any = canvas.getContext("2d");
    ctx.clearRect(0, 0, canvas.width, canvas.height);    

    const intervalId = setInterval(() => {       
        if (loadImagesCount === 0) { // When all images is ready - render
            clearInterval(intervalId); // Stop interval
            
            for (let i in allImages) {
                let imageCenterX = (canvas.width - allImages[i].image.width) / 2; // Horizontal center of canvas for image
                let imageCenterY = (canvas.height - allImages[i].image.height) / 2; // Vertical center of canvas for image
                if (allImages[i].type === "pet") { // For pet rotate ctx, place and rotate back
                    const RADIANS  = Math.PI / 180;
                    ctx.rotate(RADIANS * allImages[i].offset?.rotation);
                    ctx.drawImage(
                        allImages[i].image,
                        allImages[i].image.height < 230 ? imageCenterX - allImages[i].offset.x : imageCenterX - allImages[i].offset.alt_x,
                        allImages[i].image.height < 230 ? canvas.height - allImages[i].offset.y : canvas.height - allImages[i].offset.alt_y
                    );
                    ctx.rotate(RADIANS * allImages[i].offset?.rotation * -1);

                } else if (allImages[i].type === "mouth") { // For big mouths
                    ctx.drawImage(
                        allImages[i].image,
                        imageCenterX - allImages[i].offset.x,
                        allImages[i].image.height < 100 ? allImages[i].offset.y : allImages[i].offset.alt_y
                    )
                } else if (allImages[i].type === "body") { // For big body
                    ctx.drawImage(
                        allImages[i].image,
                        imageCenterX - allImages[i].offset.x,
                        allImages[i].image.height < 222 ? allImages[i].offset.y : allImages[i].offset.alt_y
                    )
                } else {
                    ctx.drawImage(
                        allImages[i].image,
                        imageCenterX - allImages[i].offset.x,
                        allImages[i].offset.y
                    );
                }
            }
        }
    }, 10);
}

window.addEventListener("DOMContentLoaded", () => { // Generate image on page load
    renderAvatar()
});
</script>

<template>
    <div class="generator__wrapper">
        <div class="generator__canvas-wrapper">
            <canvas class="generator__canvas" width="440" height="440"></canvas>
        </div>
        <div class="generator__buttons">
            <UButton @click="renderAvatar">Новая</UButton>
            <AvatarDownloader>
                <UButton color="red-transparent">Скачать</UButton>
            </AvatarDownloader>
        </div>
        
    </div>
</template>

<style>
.generator__wrapper {
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 12px;   
}

.generator__canvas-wrapper {
    width: 440px;
    height: 440px;
    background-color: #985CFA;
}

.generator__buttons {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    justify-content: center;
    gap: 12px;
}

@media screen and (max-width: 620px) {
    .generator__wrapper {
        width: 100%;
    }
}

@media screen and (max-width: 440px) {
    .generator__canvas-wrapper {
        width: 100vw;
        height: 100vw;
    }

    .generator__canvas {
        width: 100%;
        height: 100%;
    }

}

@media screen and (max-width: 270px) {
    .generator__buttons button {
        width: 100px;
    }
}
</style>