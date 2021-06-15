<script>
    import 'charts.css';
    import { push } from "svelte-spa-router";
let selectedParticipant = {
    part_id : sessionStorage.getItem('part_id')
};

let activities = {};

    let data;

    let activityEmotions = {
        emotion1: 0,
        emotion2: 0,
        emotion3: 0,
    };

    const getAvgOfCols = (
        array,
        start = 0,
        stop = 0,
        isReversed = undefined
    ) => {
        let counter = 0;

        const newArray = array.reduce((acc, cur) => {
            counter += stop + 1 - start;
            return [...acc, ...cur.slice(start, stop + 1)];
        }, []);

        if (isReversed != undefined)
            newArray.forEach((x, i) => {
                if (isReversed[i % isReversed.length]) {
                    newArray[i] = 6 - x;
                }
            });

        return newArray.reduce((acc, cur) => acc + cur, 0) / counter;
    };

(async function getParticipants() {
        const response = await fetch("http://localhost:3000/getparticipantactivities", {
            method: "POST", // *GET, POST, PUT, DELETE, etc.
            mode: "cors", // no-cors, *cors, same-origin
            cache: "no-cache", // *default, no-cache, reload, force-cache, only-if-cached
            credentials: "same-origin", // include, *same-origin, omit
            headers: {
                "Content-Type": "application/json",
                //'Authorization' : sessionStorage.getItem('token')
                // 'Content-Type': 'application/x-www-form-urlencoded',
            },
            redirect: "follow", // manual, *follow, error
            referrerPolicy: "no-referrer", // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
            body: JSON.stringify(selectedParticipant), // body data type must match "Content-Type" header
        }      
    )
    if (response.ok) {
            data = (await response.json()).rows;
            console.log(data);
            activities = Array.from(
                new Set(data.map((item) => item.activity_id))
            ).reduce(
                (acc, curr) => ({ ...acc, [curr]: { name: "", answers: [] } }),
                {}
            );
            data.forEach(({ activity_id, answer, activity_name }) => {
                //console.log(row);
                activities[activity_id].answers.push(answer);
                activities[activity_id].name = activity_name;
            });

            let counter = 0;

            Object.entries(activities).forEach(([, value]) => {
                const { answers } = value;

                value.emotion1 = getAvgOfCols(answers, 0, 1, [false, true]);
                value.emotion2 = getAvgOfCols(answers, 2, 3, [false, true]);
                value.emotion3 = getAvgOfCols(answers, 4, 5);
            });

            console.log(activities);

            console.log(activityEmotions);
            activityEmotions.emotion1 /= counter;
            activityEmotions.emotion2 /= counter;
            activityEmotions.emotion3 /= counter;
            console.log(activityEmotions);
        }})();


</script>
<div class="container">
    <h1>Harlog</h1>

    {#each Object.keys(activities) as activity}
        <p>{activities[activity].name}</p>
        <table id="bar-example-6" class="charts-css bar show-labels">
            <caption> Bar Example #6 </caption>
            <thead>
                <tr>
                    <th scope="col"> Year </th>
                    <th scope="col"> Progress </th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <th scope="row"> emot 1 </th>
                    <td style="--size:{activities[activity].emotion1};">
                        <span class="data"
                            >{activities[activity].emotion1.toFixed(2)}</span
                        >
                    </td>
                </tr>
                <tr>
                    <th scope="row"> emot 2 </th>
                    <td style="--size:{activities[activity].emotion2}">
                        <span class="data"
                            >{activities[activity].emotion2.toFixed(2)}</span
                        >
                    </td>
                </tr>
                <tr>
                    <th scope="row"> emot 3 </th>
                    <td style="--size:{activities[activity].emotion3}">
                        <span class="data"
                            >{activities[activity].emotion3.toFixed(2)}</span
                        >
                    </td>
                </tr>
            </tbody>
        </table>
    {/each}
</div>

<style>
    
</style>