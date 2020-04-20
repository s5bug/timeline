<style>
    .event {
        position: absolute;
        border-left: 1px solid black;
        height: calc(100vh - 3em);
    }

    .year {
        position: absolute;
        top: calc(100vh - 4em);
    }
</style>

<div class="event" style="left: {x}px; width: {w}px;">
    <div class="description">
        <slot/>
    </div>
    <div class="year">
        {year}
    </div>
</div>

<script>
    export let x;
    export let w;
    export let year;
</script>
