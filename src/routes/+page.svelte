<script>
    import { onMount } from "svelte";
    import foreground from '$lib/assets/foreground.png';
    import background from '$lib/assets/background.png';
    import { onMount as onMountSvelte } from 'svelte';

    let randomNumber;
    let randomY;
    let randomX;
    let phraseY;

    const phrases = {
        1: "Empezamos fuerte… como tu dieta.",
        2: "Esto es solo el principio. No te emociones todavía.",
        3: "Porque nunca eres mayor para esto.",
        4: "Para cuando la vida se te enrede un poco.",
        5: "Porque cuidarte también es quererte.",
        6: "Sí. Te conocemos demasiado.",
        7: "No todo iba a ser glamour 😈",
        8: "Un recuerdo que merece estar a la vista.",
        9: "Porque no todo lo bonito es para guardar… algunas cosas se comen.",
        10: "Tu lado friki también es parte de ti.",
        11: "Un poco de mimo nunca sobra.",
        12: "Para crear, desconectar y perder el tiempo bonito.",
        13: "Porque siempre hay historias que merecen ser leídas.",
        14: "Seguimos. Y todavía queda lo mejor.",
        15: "Este no es para hoy. Es para cuando lo necesites.",
        16: "Para cuidar algo que usas cada día… y que siempre te acompaña.",
        17: "Un olor que ojalá te recuerde a casa.",
        18: "Porque incluso lo oscuro puede ser bonito.",
        19: "Algo pequeño, pero pensado para ti.",
        20: "Hay canciones que cuentan historias mejor que las palabras.",
        21: "Este ya empieza a ser serio.",
        22: "Algunas cosas no se regalan por casualidad.",
        23: "Esto no es un regalo. Es un plan.",
        24: "Aquí están tus recuerdos, pero sobre todo estamos nosotros. Las personas que te han visto crecer, reír, caer y levantarte.",
        25: "¡Sorpresa!"
    }

    onMountSvelte(() => {
        // Generar número random y posición Y random (mínimo 35% hacia abajo)
        setTimeout(() => {
            const getParams = new URLSearchParams(window.location.search);
            const numberParam = getParams.get('number');
            randomNumber = numberParam ? parseInt(numberParam) : '99';
            const minY = window.innerHeight * 0.35;
            const maxY = window.innerHeight * 0.8;
            randomY = Math.floor(Math.random() * (maxY - minY) + minY);
            const minX = window.innerWidth * 0.1;
            const maxX = window.innerWidth * 0.9;
            randomX = Math.floor(Math.random() * (maxX - minX) + minX);
        }, 800);
        
        
    });

    onMount(async () => {
        await import('scratchcard-js').then(({ ScratchCard, SCRATCH_TYPE }) => {
            const scContainer = document.getElementById('js--sc--container');
            const sc = new ScratchCard(scContainer, {
                scratchType: SCRATCH_TYPE.LINE,
                containerWidth: window.innerWidth,
                containerHeight: window.innerHeight,
                imageForwardSrc: foreground,
                imageBackgroundSrc: background,
                htmlBackground: '',
                clearZoneRadius: 40,
                nPoints: 0,
                pointSize: 0,
                enabledPercentUpdate: true,
                percentToFinish: 60,
                callback: function () {}
            });
            sc.init();
        });
    });
</script>

 <div class="sc__wrapper relative">
    <!-- scratchcard -->
    <div id="js--sc--container" class="sc__container">
        <!-- background image insert here by scratchcard-js -->
        <!-- canvas generate here -->
        {#if randomNumber !== undefined && randomY !== undefined && randomX !== undefined}
            <div class="random-number" style="position:absolute; left:{randomX}px; transform:translateX(-50%); top:{randomY}px; font-size:3rem; color:#fff; font-weight:bold; text-shadow:2px 2px 8px #000; pointer-events:none; z-index:0;">
                {randomNumber}
            </div>
        {/if}
        {#if randomNumber !== undefined}
            <div class="absolute w-full flex justify-center items-center h-[35vh] px-[20px] text-center" >
                {phrases[randomNumber]}
            </div>
        {/if}
    </div>
    <!-- infos -->
    <div class="sc__infos">
        <!-- percent -->
    </div>
</div>

<style>
    :global(.sc__container img) {
        position: absolute;
        width: 100vw;
        height: 100vh;
        z-index: -1;
    }
    :global(.sc__container canvas) {
        position: absolute;
        width: 100vw;
        height: 100vh;
        z-index: 2;
    }
</style>