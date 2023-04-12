<script>
    import {FeedbackStore} from '../stores'

    $: count = $FeedbackStore.length
	$: average = $FeedbackStore.reduce((a, {rating})=> a + rating, 0) / $FeedbackStore.length
</script>

<div class="feedback-stats">
    <h4>{count} Reviews</h4>
    {#if isNaN(average)}
    <h4>Ratings Average: 0</h4>
    {:else}
    <h4>Ratings Average: {average}</h4>
    {/if}


</div>

<style>
    .feedback-stats {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
</style>