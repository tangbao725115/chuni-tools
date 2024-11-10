<script lang="ts">
    import { floorToFixed } from "@/common/number"


    export let records: ParsedRecord[]

    function calculateTotalOp(records: ParsedRecord[]) {
        const getOpPerSong = records.reduce((acc, curr) => {
            // Check if the stage already has a recorded score or if the current score is higher
            if (!acc[curr.title] || curr.op > acc[curr.title]) {
                acc[curr.title] = curr.op;
            }
            return acc;
        }, {} as Record<string, number>);

        // Sum the highest scores across all stages
        return Object.values(getOpPerSong).reduce((sum, score) => sum + score, 0);
    }

    function calculateMaxOp(records: ParsedRecord[]) {
        const getMaxOpPerSong = records.reduce((acc, curr) => {
            // Check if the stage already has a recorded score or if the current score is higher
            if (!acc[curr.title] || curr.opMax > acc[curr.title]) {
                acc[curr.title] = curr.opMax;
            }
            return acc;
        }, {} as Record<string, number>);

        // Sum the highest scores across all stages
        return Object.values(getMaxOpPerSong).reduce((sum, score) => sum + score, 0);
    }

    $: totalOverpower = calculateTotalOp(records)
    $: maxOverpower = calculateMaxOp(records)
    $: progress = (totalOverpower / maxOverpower) * 100
</script>

<div
    class="wrapper"
    class:aj={maxOverpower && totalOverpower.toFixed() === maxOverpower.toFixed()}>
    <div>
        <div class="op-total">{floorToFixed(totalOverpower / 10000, 2)}</div>
        <div class="op-max">&#xFF0F;{(maxOverpower / 10000).toFixed(1)}</div>
    </div>
    <div class="progress">
        <div style="width:{progress}%">
            {isNaN(progress) ? "--" : floorToFixed(progress, 3)}%
        </div>
    </div>
</div>

<style lang="sass">
    .wrapper
        border: var(--theme-border) 3px solid
        border-radius: 0.5rem
        background-color: var(--theme-bg-sub)
        box-sizing: border-box
        width: 100%
        max-width: 600px
        padding: 0.5rem 2rem
        margin: 0.5rem
        display: flex
        justify-content: space-evenly
        align-items: center
        text-align: center
        gap: 1rem
        overflow-x: scroll
        white-space: nowrap
        &::-webkit-scrollbar
            display: none
        &.aj .progress, &.aj .op-total
            color: var(--theme-clear-aj)
            text-shadow: 0 0 10px var(--theme-clear-aj)
    .op-total
        font-size: 1.5em
        font-weight: bold
    .op-max
        font-size: .8em
        color: var(--theme-text-dim)
    .progress
        width: 100%
        border-radius: 1em
        background-color: var(--theme-bg-main)
        div
            border-radius: 1em
            background-color: var(--theme-control)
            font-weight: bold
</style>
