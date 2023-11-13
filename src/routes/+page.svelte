<script>
    const certLevels = [{value: "A", text: "A"}, {value: "B", text: "B"}, {value: "C", text: "C"}, {value: "D", text: "D"}, {value: "E", text: "E - Elev"}]
    let cert = "";
    let certLevel = "A";
    let name = "";
    let email = "";
    let tel = "";
    let hmember = false;
    let applied = false;
    
    $: swishUrl = "https://app.swish.nu/1/p/sw/?sw=1235425467&amt=" + (hmember ? "2100" : "3000") + "&cur=SEK&msg=" + cert + "&src=qr"

    $: postData = "entry.2040625706=" + encodeURIComponent(cert) + "&entry.1366402579=" + encodeURIComponent(name) + "&entry.1208613624=" + encodeURIComponent(email) + "&entry.204702185=" + encodeURIComponent(tel) + "&entry.79715208=" + encodeURIComponent(certLevel)
    /**
	 * @type {any}
	 */
    let dialog;
    $: if (dialog && applied) dialog.showModal();

    async function apply() {
        applied = true;
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
    <h1>Medlemsskapsansökan</h1>
    <label for="cert">Certnummer:</label>
    <input id="cert" type="text" bind:value={cert} placeholder="Certnummer" autocomplete="off" />
    <label for="certlevel">Licensegrad:</label>
    <select id="certlevel" bind:value={certLevel}>
		{#each certLevels as level}
            <option value={level.value}>
				{level.text}
			</option>
		{/each}
	</select>
    <label for="name">Namn:</label>
    <input id="name" type="name" bind:value={name} placeholder="Namn" autocomplete="name" />
    <label for="email">Email:</label>
    <input id="email" type="email" bind:value={email} placeholder="Email" autocomplete="email" />
    <label for="tel">Telefon:</label>
    <input id="tel" type="tel" bind:value={tel} placeholder="Telefon" autocomplete="tel" />
    <div class="hmember">
        <label id="label-hmember" for="hmember">Hedersmedlem:</label>
        <input id="hmember" type="checkbox" bind:checked={hmember}/>
    </div>
    <button on:click={apply}>Ansök</button>
    <!-- svelte-ignore a11y-click-events-have-key-events a11y-no-noninteractive-element-interactions -->
    <dialog bind:this={dialog} on:close={() => (applied = false)}	on:click|self={() => dialog.close()}>
        <!-- svelte-ignore a11y-no-static-element-interactions -->
        <div on:click|stopPropagation>
            <h1>Ansökan mottagen</h1>
            <p>För att medlemsskapet skall godkännas så behöver medlemsavgiften på {hmember ? "2100" : "3000"} kr skickas till antingen Swishnummer&nbsp;1235425467 eller Bankgironummer&nbsp;117-0224.</p>
            <p>Ange certnummer som meddelande.</p>
            <img src={hmember ? "swish_2100.svg" : "swish_3000.svg"} alt="Swish QR Code" width="300px" height="300px" />
            <a href={swishUrl}><button>Öppna Swish på denna enhet</button></a>
        </div>
    </dialog>
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
    .centered {
        font-family: 'Roboto', sans-serif;
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
    .hmember {
        width: 100%;
        display: inline-block;
    }
    #label-hmember {
        width: 0%;
        padding: 5px 0px 0px 5px;
        margin: 10px, 0px, 0px, 0px;
        display: inline-flex;
        float: left;
    }
    #hmember {
        width: 80%;
        height: 100%;
        padding: 5px, 0px, 0px, 0px;
        margin: 10px, 0px, 0px, 0px;
        display: inline-flex;
        float: right;
        accent-color: #00DCE7;
    }
    input, select, option {
        width: 100%;
        padding: 12px 20px;
        margin: 8px 0;
        display: inline-block;
        border: 1px solid #ccc;
        border-radius: 4px;
        box-sizing: border-box;
        background-color: #FFFFFF;
    }
    input[type="text"]:focus, input[type="tel"]:focus, input[type="email"]:focus, input[type="name"]:focus, select:focus {
        outline: #00DCE7 solid 2px;
    }
    button {
        font-family: 'Roboto', sans-serif;
        font-size: medium;
        width: 100%;
        background-color: #00DCE7;
        padding: 14px 20px;
        margin: 8px 0;
        border: none;
        border-radius: 4px;
        cursor: pointer;
    }
    button:hover {
        background-color: #01cad4; /* #0090c6; */
    }
    h1, p, a {
        text-align: center;
        width: 100%;
        padding: 0px 5px;
        margin: 15px 0px;
        display: inline-block;
    }
    img {
        width: 100%;
        padding: 10px 10px;
        display: inline-block;
    }
    dialog {
        font-family: 'Roboto', sans-serif;
        max-width: 30em;
        margin: 0 auto;
        border-radius: 5px;
        background-color: #f2f2f2;
        background: rgba(255, 255, 255);
		border: none;
        padding: 20px;
	}
	dialog::backdrop {
		background: rgba(168, 168, 168, 0.205)
    }
	dialog > div {
        font-family: 'Roboto', sans-serif;
        max-width: 30em;
        margin: 0 auto;
        padding: 5px;
	}
	dialog > div > a > button {
        font-family: 'Roboto', sans-serif;
        color: #ffffff;
        width: 50%;
        background-color: #C1347A;
    }
	dialog > div > a > button:hover {
        background-color: #D53A53;
    }
</style>