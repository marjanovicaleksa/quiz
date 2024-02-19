<script>
    import {onMount} from 'svelte';
    export let question;
    export let correctAnswer;
    export let incorrectAnswers;

    const shuffle = (array) => array.sort(()=>Math.random()-0.5);
    let possibleAnswers = [...incorrectAnswers, correctAnswer];

    let correctIndex = possibleAnswers.indexOf(correctAnswer);

    const handleClick = (answer) =>{
        if (answer===correctAnswer){
            alert("Correct!");
        }
        else{
            alert("Wrong! The correct answer is "+correctAnswer+".");
        }
    }

    const getQuestion = async()=>{
        const res = await fetch('https://opentdb.com/api.php?amount=1&difficulty=easy&type=multiple');
        let data = await res.json();
        question = data.results[0].question;
        console.log(data);
    }
    onMount(getQuestion);
</script>
<div>
    <h3>{@html question}</h3>
    <div id="answers">
        <!--prikazi sve ponudjene kao button-e-->
        {#each possibleAnswers as pa, i}
            <button on:click={()=>handleClick(pa)}>{pa}</button>

        {/each}
    </div>
</div>