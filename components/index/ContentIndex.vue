<template>
    <div class="content">
        <div class="content-background">
            <p class="content-title">Title</p>
            <p class="content-text">Text</p>
            <button class="content-button">Botão do Deuses</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ContentIndex',

}
</script>

<style scoped>
.content {
    grid-area: content;
    
    padding: 8vh 4vw 8vh 6vw;
}
.content-background {
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;

    height: 100%;
    border-radius: 3vh;
    background: var(--baseColor);
    box-shadow: 1px 1px 15px -5px var(--lightColor);
}
.content-button {
    display: flex;
    justify-content: center;
    align-items: center;

    background: var(--baseColor);
    box-shadow: 1px 1px 15px -5px var(--lightColor);
    border-radius: 1vh;
    border: 2px solid var(--textColor);
    height: 5vh;
    padding: 2vh;
    transition: background 1s;
}
.content-button:hover {
    background: var(--textColor);
    color: var(--baseColor);
}
</style>