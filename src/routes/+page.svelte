<script>
    const certLevels = [{value: "A", text: "A"}, {value: "B", text: "B"}, {value: "C", text: "C"}, {value: "D", text: "D"}, {value: "E", text: "E - Elev"}]
    let cert = "";
    let certLevel = "A";
    let name = "";
    let email = "";
    let tel = "";
    let nameRel = "";
    let telRel = "";
    let hmember = false;
    
    $: swishUrl = "https://app.swish.nu/1/p/sw/?sw=1235425467&amt=" + (hmember ? "2100" : "3000") + "&cur=SEK&msg=" + cert + "&src=qr"

    $: postData = "entry.2040625706=" + encodeURIComponent(cert) + "&entry.1366402579=" + encodeURIComponent(name) + "&entry.1208613624=" + encodeURIComponent(email) + "&entry.204702185=" + encodeURIComponent(tel) + "&entry.79715208=" + encodeURIComponent(certLevel) + "&entry.1888244923=" + encodeURIComponent(nameRel) + "&entry.783511263=" + encodeURIComponent(telRel)
    /**
	 * @type {any}
	 */
    let dialog;

    async function apply() {
        dialog.showModal();
		const res = await fetch("https://docs.google.com/forms/u/0/d/e/1FAIpQLSeUVFeWKF0aPC-ZzD_hu7-_RA2EwD_DslduLWBJDodc9K-dBQ/formResponse", {
			method: "POST",
            headers: { "Content-Type": "application/x-www-form-urlencoded"},
			body: postData
		})/*
        if (res.ok && res.status >= 200 && res.status < 300) {
            
        } else {
            alert("Tekniskt fel! :( Pröva att ladda om sidan, annars så leta hjälp.");
        }*/
    }
</script>

<div class="centered">
    <!-- svelte-ignore a11y-click-events-have-key-events a11y-no-noninteractive-element-interactions -->
    <dialog bind:this={dialog} on:click|self={() => dialog.close()}>
        <!-- svelte-ignore a11y-no-static-element-interactions -->
        <div on:click|stopPropagation>
            <h1>Ansökan mottagen</h1>
            <p>För att medlemsskapet skall godkännas så behöver medlemsavgiften på {hmember ? "2100" : "3000"} kr skickas till antingen Swishnummer&nbsp;1235425467 eller Bankgironummer&nbsp;117-0224.</p>
            <p>Ange certnummer som meddelande.</p>
            <img src={hmember ? "swish_2100.svg" : "swish_3000.svg"} alt="Swish QR Code" width="300px" height="300px" />
            <a href={swishUrl}><button>Öppna Swish på denna enhet</button></a>
        </div>
    </dialog>
    <h1>Medlemsskap 2024</h1>
    <label for="cert">Certnummer:</label>
    <input id="cert" type="text" bind:value={cert} placeholder="Certnummer" autocomplete="off" />
    <label for="certlevel">Licensegrad:</label>
    <select id="certlevel" bind:value={certLevel}>
		{#each certLevels as level}
            <option id={level.value} value={level.value}>
				<label for={level.value} class="level">{level.text}</label>
			</option>
		{/each}
	</select>
    <label for="name">Namn:</label>
    <input id="name" type="name" bind:value={name} placeholder="Namn" autocomplete="name" />
    <label for="email">Email:</label>
    <input id="email" type="email" bind:value={email} placeholder="Email" autocomplete="email" />
    <label for="tel">Telefon:</label>
    <input id="tel" type="tel" bind:value={tel} placeholder="Telefon" autocomplete="tel" />
    <div id="hmember-div">
        <label id="hmember-label" for="hmember">Hedersmedlem:</label>
        <input id="hmember" type="checkbox" bind:checked={hmember}/>
    </div>
    <div id="rel-div">
        <h2>Närmast anhörig</h2>
        <input id="nameRel" type="name" bind:value={nameRel} placeholder="Namn" autocomplete="name" />
        <input id="telRel" type="tel" bind:value={telRel} placeholder="Telefon" autocomplete="tel" />
    </div>
    <button on:click={apply}>Ansök</button>
</div>

<style>
    /*
        #33AAFF - default
        #00C6FF - light blue / sky
        #00DCE7 - light teal
    Matching Gradient
        #33AAFF - Base
        #6A94E8

        #00c6ff - Base
        #3facfa
        #6e8fe9
        #916fcb
        #a74ca1
        #ac246f
    Spot Palette
        #00c6ff
        #0090c6
        #c8f8ff
        #773300
    */
    * {
        color: #000000;
        font-size: 1em;
        font-family: 'Roboto', sans-serif;
    }
    .centered {
        max-width: 30em;
        margin: 0 auto;
        border-radius: 5px;
        background-color: #f2f2f2;
        padding: 20px;
    }
    label {
        width: 100%;
        padding: 0px 5px;
        display: inline-block;
    }
    #hmember-div {
        display: flex;
        justify-content: space-between;
        width: 100%;
        margin: 0px 0px 15px 0px;
    }
    #hmember-label {
        width: 100%;
        padding: 5px 0px 0px 5px;
        margin: 5px 0px 0px 0px;
    }
    #hmember {
        width: 100%;
        accent-color: #00DCE7;
        padding: 10px 0px 0px 5px;
        margin: 12px 0px 0px 100px;
    }
    input {
        width: 100%;
        padding: 12px 20px;
        margin: 8px 0;
        display: inline-block;
        border: 1px solid #ccc;
        border-radius: 4px;
        box-sizing: border-box;
        background-color: #FFFFFF;
    }
    select {
        width: 100%;
        padding: 12px 20px;
        margin: 8px 0; 
        display: inline-block;
        border: 1px solid #ccc;
        border-radius: 4px;
        box-sizing: border-box;
        background-color: #FFFFFF;
    }
    option {
        width: 100%;
        padding: 0;
        margin: 0;
        display: inline-block;
        border: 1px solid #ccc;
        border-radius: 4px;
        box-sizing: border-box;
        background-color: #FFFFFF;
    }
    .level {
        width: 100%;
        padding: 12px 20px;
        margin: 8px 0;
        display: inline-block;
        border: 1px solid #ccc;
        border-radius: 4px;
        box-sizing: border-box;
        background-color: #FFFFFF;
    }
    #rel-div {
        border-radius: 5px;
        background-color: #cacaca;
        margin: 0 auto;
        padding: 10px;
    }
    input[type="text"]:focus, input[type="tel"]:focus, input[type="email"]:focus, input[type="name"]:focus, select:focus {
        outline: #00DCE7 solid 2px;
    }
    button {
        width: 100%;
        background-color: #00DCE7;
        padding: 14px 20px;
        margin: 12px 0;
        border: none;
        border-radius: 4px;
        cursor: pointer;
    }
    button:hover, button:focus {
        background-color: #01cad4; /* #0090c6; */
        outline: none;
    }
    h1 {
        font-size: 2em;
    }
    h1, p, a {
        text-align: center;
        width: 100%;
        padding: 0px 5px;
        margin: 15px 0px;
        display: inline-block;
    }
    h2 {
        font-size: 1.2em;
        margin: 0px 0px 0px 0px;
    }
    img {
        width: 100%;
        padding: 10px 10px;
        display: inline-block;
    }
    dialog {
        max-width: 30em;
        margin: 0px auto 0px auto;
        border-radius: 5px;
        background-color: #f2f2f2;
        background: rgba(255, 255, 255);
		border: none;
        outline: none;
        padding: 20px;
	}
    dialog:focus {
        outline: none;
    }
	dialog::backdrop {
		background: rgba(168, 168, 168, 0.205)
    }
	dialog > div {
        max-width: 30em;
        margin: 0 auto;
        padding: 5px 5px;
	}
	dialog > div > a {
        outline: none;
	}
	dialog > div > a > button {
        color: #ffffff;
        width: 50%;
        background-color: #C1347A;
    }
	dialog > div > a > button:hover {
        background-color: #D53A53;
        outline: none;
    }
	dialog > div > a > button:focus {
        background-color: #D53A53;
        outline: none;
    }
</style>