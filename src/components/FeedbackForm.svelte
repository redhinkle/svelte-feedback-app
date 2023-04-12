<script>
import {FeedbackStore} from '../stores'
import {v4 as uuidv4} from 'uuid'
import Card from "./Card.svelte";
import Button from "../Button.svelte";
import RatingSelect from "./RatingSelect.svelte";

let text = ''
let rating = 10
let btnDisabled = true
let min = 10
let message

const handleInput = () =>{
    if(text.trim().length < min){
        message = `Text must be at least ${min} characters`
        btnDisabled = true
    } else{
        message = null
        btnDisabled = false
    }
}

const handleSelect = e => rating = e.detail

const handleSubmit = () =>{
    if(text.trim().length > min){
        const newFeedback = {
            id: uuidv4(),
            text,
            rating: +rating
        }
        FeedbackStore.update((currentFeedback)=>{
            return [newFeedback, ...currentFeedback]
        })
        text=''
    }
}
</script>

<Card>
    <header>
        <h2>How would you rate your service with us?</h2>
    </header>
    <form on:submit|preventDefault={handleSubmit}>
        <RatingSelect on:rating-select={handleSelect}/>
        <div class="input-group">
            <input type="text"  on:input={handleInput} bind:value = {text} placeholder="Tell us something that keeps you coming back">
            <Button disabled={btnDisabled} type="submit">Send</Button>
        </div>
        {#if message}
            <div class="message">
                {message}
            </div>
        {/if}
    </form>
</Card>

<style>
    header{
        max-width: 400px;
        margin: auto;
    }

    header h2{
        color: #333;
        text-align: center;
    }

    .input-group{
        border: 1px solid #333;
        border-radius: 8px;
        display: flex;
        justify-content: space-between;
        padding: 8px;
    }

    input[type="text"]{
        border: none;
        background: none;
        width: 80%;
    }
    input[type="text"]:focus{
        outline: none;
    }

</style>