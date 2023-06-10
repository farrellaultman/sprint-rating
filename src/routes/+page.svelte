<!-- Scripts
–––––––––––––––––––––––––––––––––––––––––––––––––– -->
<script>

    import RangeSlider from "svelte-range-slider-pips";

    let sloppy = 'Yes';
    let edc = 'No';
    let sprintGoal = 'No';
    let overallPersonalRating = [1];
    let overallTeamRating = [1];
    let extraordinary = 'No';

    let points = 0;
    $: {
        points = 0;

        // Positive Metrix
        points += parseFloat(overallPersonalRating) / 5 * 0.45
        points += parseFloat(overallTeamRating) / 5 * 0.45
        if (sprintGoal == 'Yes') points += 1 * 0.10;

        // Penalties
        if (sloppy == 'Yes') points -= 1 * 0.20;
        if (edc == 'No') points -= 1 * 0.20;

        // Bonus
        if (extraordinary == 'Yes') points += 1 * 0.20;

        // Scale to 5
        points *= 5;

        // Min value for points is 1
        if (points < 1) points = 1;

        // One decimal place only
        points = points.toFixed(1);
    };

    // Clamp the rating between 1 and 5
    $: overall_rating_rounded = Math.max(Math.min(Math.round(points), 5), 1);
    const stars = ['★☆☆☆☆', '★★☆☆☆', '★★★☆☆', '★★★★☆', '★★★★★'];
    $: star_rating = stars[overall_rating_rounded - 1];
</script>


<!-- Page Layout
–––––––––––––––––––––––––––––––––––––––––––––––––– -->
<main class="container">

<div class="content">
    <h1>Sprint Rating</h1>
    <p>Answer the following questions to calculate your overall rating for the sprint.</p>

    <p>
        <small>Were you sloppy or allow a TM to be sloppy during this sprint?</small>
        <label>
            <input type=radio bind:group={sloppy} name='sloppy' value={'Yes'}>
            Yes
        </label>
        <label>
            <input type=radio bind:group={sloppy} name='sloppy' value={'No'}>
            No
        </label>
    </p>

    <p>
        <small>Did you make Every Day Count during the sprint?</small>
        <label>
            <input type=radio bind:group={edc} name='edc' value={'Yes'}>
            Yes
        </label>
        <label>
            <input type=radio bind:group={edc} name='edc' value={'No'}>
            No
        </label>
    </p>

    <p>
        <small>Did you complete a Sprint goal?</small>
        <label>
            <input type=radio bind:group={sprintGoal} name='sprintGoal' value={'Yes'}>
            Yes
        </label>
        <label>
            <input type=radio bind:group={sprintGoal} name='sprintGoal' value={'No'}>
            No
        </label>
    </p>

    <p class="range-slider">
        <small>On a scale of 1 to 5, what is your overall sprint rating for yourself?</small>
        <RangeSlider bind:values={overallPersonalRating} min={1} max={5} all={'label'} springValues={{ stiffness: 0.4, damping: 1 }} pips />
    </p>

    <p class="range-slider">
        <small>On a scale of 1 to 5, what is your overall sprint rating for team?</small>
        <RangeSlider bind:values={overallTeamRating} min={1} max={5} all={'label'} springValues={{ stiffness: 0.4, damping: 1 }} pips />
    </p>

    <p>
        <small>Did you do something extraordinary during this sprint?</small>
        <label>
            <input type=radio bind:group={extraordinary} name='extraordinary' value={'Yes'}>
            Yes
        </label>
        <label>
            <input type=radio bind:group={extraordinary} name='extraordinary' value={'No'}>
            No
        </label>
    </p>
</div>

<h3 class="footer">Overall rating: {points} {star_rating}</h3>

</main>


<!-- Page Layout
–––––––––––––––––––––––––––––––––––––––––––––––––– -->
<style>

.content {
    padding-bottom: 80px;
}

.footer {
  position: fixed;
  left: 0;
  bottom: 0;
  width: 100%;
  background-color: black;
  color: var(--primary);
  text-align: center;
  margin-bottom: 0;
  margin-top: 5rem;
}

.range-slider {
--range-slider:          #d7dada; /* slider main background color */
--range-handle-inactive: var(--primary); /* inactive handle color */
--range-handle:          var(--primary); /* non-focussed handle color */
--range-handle-focus:    var(--primary); /* focussed handle color */
}

</style>