<script>
    import {Howl, Howler} from 'howler';

    let meow = new Howl({
        src: ['meow2.mp3'],
        volume: 0.5,
        html5: true,
    });
    let angryMeow = new Howl({
        src: ['angry-meow.mp3'],
        volume: 0.5,
        html5: true,
    });
    


    let questions = $state([])
	questions = [
        {
            question: "Quelle est le meilleurs brainrot italien",
            choixDeReponse: [
                "(a) tralalelo tralala",
                "(b) bombardilo crocodilo",
                "(c) tripitropa",
                "(d) tung tung tung tung tung tung tung tung tung tung sahur",
            ],
            indexBonneReponse: 1,
            corriger: "la bonne reponse etait (b)"
        },
        {
            question: "Quelle est le lgbt.... le plus a jours",
            choixDeReponse: [
                "(a) lgbtq",
                "(b) lgbtqia+",
                "(c) lgbtqrstuv",
                "(d) 2slgbtqia+",
            ],
            indexBonneReponse: 3,
            corriger: "la bonne reponse etait (d) parce que c'est ca "
        },
        {
            question: "C'est qui la meilleur porte logique",
            choixDeReponse: [
                "NAND",
                "OR",
                "NOR",
                "NOT",
            ],
            indexBonneReponse: 0,
            corriger: "NAND >>>>>>"
        },
        {
            question: "Quelle est le complement orthogonal a l'espace des ligne?",
            choixDeReponse: [
                "C(A^t)",
                "C(A)",
                "N(A)",
                "N(A^t)",
            ],
            indexBonneReponse: 2,
            corriger: "L'espace des ligne est orthogonal au noyau."
        },
        {
            question: "C'est qui le meilleur philosophe de tout les temps",
            choixDeReponse: [
                "Terry Davis",
                "Platon",
                "Aristote",
            ],
            indexBonneReponse: 2,
            corriger: "Aristote ofc"
        },
    ]

    let joueur = $state({})
    joueur = {
        selectedChoice: 0,
        indexQuestion: 0,
        nbBonneReponse: 0,
        nbMauvaiseReponse: 0,
        listIndexQuestionReussi: [

        ],
    }
    
    let corrigerState = $state(false)
    let endState = $state(false)


    const handleSelect = (e, i) => {
        if (e.target.checked) {
            joueur.selectedChoice = i;
        } else {
            joueur.selectedChoice = null;
        }
        console.log("hit")
    }


    const handleSendResponse = () => {
        console.log("handleSendResponse hit")
        if (joueur.selectedChoice == questions[joueur.indexQuestion].indexBonneReponse) {
            meow.play()
            joueur.nbBonneReponse++
            joueur.listIndexQuestionReussi.push(joueur.indexQuestion)
            corrigerState = true
            return
        }
        angryMeow.play()
        joueur.nbMauvaiseReponse++
        corrigerState = true
    }
    
    const handleOk = () => {
        console.log("handleOk hit")
        if (joueur.indexQuestion == (questions.length - 1)) {
            endState = true
            corrigerState = false
            return
        }
        joueur.indexQuestion++
        corrigerState = false
    }

    const handleRestart = () => {
        console.log("handleRestart hit")
        joueur = {
            selectedChoice: 0,
            indexQuestion: 0,
            nbBonneReponse: 0,
            nbMauvaiseReponse: 0,
            listIndexQuestionReussi: [

            ],
        }
        endState = false
    }
</script>
<div class="page">
    <div class="explication">
        Vous êtes un chat ! Le but du jeu est d'aller informer toutes les maisons à propos des bienfaits de l'EDI. 
        Il y aura plusieurs questions. Si vous y répondez correctement, vous aurez réussi à informer correctement la maison. 
        Sinon, vous échouerez. Le but est de réussir à bien informer toutes les maisons !

    </div>
    <div class="qContainer">
        <div class="question">
            {questions[joueur.indexQuestion].question}
        </div>

        <div class="choixContainer">
        {#if endState}
            <div> Vous avez completer votre mission avec un reultat de : {joueur.nbBonneReponse}/{questions.length}</div>
        {:else if corrigerState}
            {#if joueur.listIndexQuestionReussi.includes(joueur.indexQuestion)}
                <div>Bravo ! Vous avez reussi</div>
            {:else}
                <div>Ayayayyy. Vous avez rater.</div>
            {/if}
            {questions[joueur.indexQuestion].corriger}
        {:else}
            {#each questions[joueur.indexQuestion].choixDeReponse as choix, i}
                <label class="choix">
                    <input type="checkbox" checked={joueur.selectedChoice === i} onchange={(e)=>handleSelect(e, i)}/>
                    {choix}
                </label>
            {/each}
        {/if}
        </div>
        <div class="bonneReponse">
            
        </div>
        <div class="buttomBar">
            <div class="stats">
                <span>Maison reussi : {joueur.nbBonneReponse}</span>
            </div>
            {#if endState}
            <button class="envoyer" onclick={handleRestart}>
                Recommencer ?
            </button>
            {:else if corrigerState}
            <button class="envoyer" onclick={handleOk}>
                Ok !
            </button>
            {:else}
            <button class="envoyer" onclick={handleSendResponse}>
                Envoyer
            </button>
            {/if}
        </div>
    </div>
</div>

<div class="bar">
    {#each questions as question, i}
         <div class="q">
            <div class="sign">
                <div class="textSign">
                    {i+1}
                </div>
            </div>
         </div>
    {/each}
    <img src="/cat3.gif" alt="" class="catGif" style="left: calc({(((joueur.indexQuestion + 1) / (questions.length) ))*100 - 1/(questions.length*2)*100}% - 2em); /* Center the cat in 1/6 of the page width */">
</div>

<style>
    .page {
        flex: 1;
        display: flex;
        justify-content: space-around;
        align-items: center;
        flex-direction: column;
        background-image: url("/sky3.jpg");
        /* background-size: cover; */
        /* background-attachment: fixed; */
    }
    .explication {
        margin-inline: 10em;
        color: whitesmoke;
        font-size: 1.2em;
        background-color: rgba(0, 0, 0, 0.49);
        padding: 1em;
        border-radius: 0.5em;
    }
    .qContainer {
        background-color: grey;
        border-radius: 1em;
        width: 40em;
        height: 20em;
        display: flex;
        flex-direction: column;
        padding: 1em;
    }
    .question {
        background-color: rgb(196, 196, 196);
        border-radius: 0.5em;
        padding: 1em;
    }
    .choixContainer {
        flex: 1;
        margin-top: 1em;
        padding: 1em;
        background-color: rgb(196, 196, 196);
        border-radius: 0.5em;
        display: flex;
        flex-direction: column;
        gap: 1em; /* Add space between each element */
    }
    .buttomBar {
        display: flex;
        justify-content: center;
    }
    .envoyer {
        all: unset;
        cursor: pointer;
        background-color: rgb(196, 196, 196);
        margin-top: 1em;
        padding: 1em;
        border-radius: 0.5em;
        width: 10em;
        text-align: center;
        transition: 0.3s;
    }
    .envoyer:hover {
        background-color: rgb(235, 235, 235)
    }
    .envoyer:active {
        transform: scale(0.90);
    }

    .stats {
        margin-top: 1em;
        margin-right: 1em;
        border-radius: 0.5em;
        padding: 1em;
        background-color: rgb(196, 196, 196);
        flex: 1;
        display: flex;
        align-items: center;
    }



    
    .catGif {
        position: absolute;
        height: 6em;
        /* left: calc(16.67% - 4em); Center the cat in 1/6 of the page width */
        top: 50%; /* Center vertically */
        transform: translateY(-20%); /* Adjust for the height of the cat */
        transition: 1s;
    }
    .catGif:hover {
        left: calc(50% - 4em); /* Center the cat in 1/6 of the page width */
    }
    .bar {
        display: flex;
        background-color: bisque;
        position: relative;
        justify-content: space-around;
        align-items: center;
        background-image: url("/grass.png");
        height: 18em;
    }
    .q {
        background-image: url("/house.png");
        background-size: 6em;
        background-position: top right;
        background-repeat: no-repeat;
        height: 10em;
        width: 11em;
        display: flex;
        color: black;
        font-size: 1em;
    }
    .sign {
        font-size: 2em;
        width: 2em;
        background-image: url("/sign.png");
        background-repeat: no-repeat;
        background-size: 2em;
        background-position: top;
        display: flex;
    }
    .textSign {
        margin-top: 0.3em;
        margin-left: 0.9em;
        font-size: 0.8em;
    }
</style>