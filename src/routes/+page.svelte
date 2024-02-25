<script>
    const certLevels = [{value: "A", text: "A"}, {value: "B", text: "B"}, {value: "C", text: "C"}, {value: "D", text: "D"}, {value: "E", text: "E - Elev"}]
    const memberType = [{value: "Medlem", text: "Medlem"}, {value: "Hedersmedlem", text: "Hedersmedlem"}, {value: "Stödmedlem", text: "Stödmedlem"}]
    let cert = "";
    let certLevel = "A";
    let pnr = "";
    let name = "";
    let email = "";
    let tel = "";
    let nameRel = "";
    let telRel = "";
    let member = "Medlem";
    
    $: memberPrice =  (member == "Hedersmedlem" ? "2100" : member == "Stödmedlem" ? "300" : "3000");

    $: swishUrl = "swish://payment?data={\"version\":1,\"payee\":{\"value\":\"1235425467\"},\"amount\":{\"value\":" + memberPrice + "},\"message\":{\"value\":\"" + cert + "\"}}";
    //             entry.2040625706=                      446666    &entry.1416888566=                   76767567    &entry.1366402579=                       Berif    &entry.1208613624=              fdsf%40gszc.azs    &entry.204702185=                   53452435    &entry.1888244923=                         bhdgfg    &entry.783511263=                       6457457    &entry.79715208=                                D
    $: postData = "entry.2040625706=" + encodeURIComponent(cert) + "&entry.1416888566=" + encodeURIComponent(pnr) + "&entry.1366402579=" + encodeURIComponent(name) + "&entry.1208613624=" + encodeURIComponent(email) + "&entry.204702185=" + encodeURIComponent(tel) + "&entry.1888244923=" + encodeURIComponent(nameRel) + "&entry.783511263=" + encodeURIComponent(telRel) + "&entry.79715208=" + encodeURIComponent(certLevel);
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
		}).then((res) => {
            /*
            if (res.ok && res.status >= 200 && res.status < 300) {
                dialog.showModal();
            } else {
                alert("Tekniskt fel! :( Pröva att ladda om sidan, annars så leta hjälp.");
            }
            */
        });
    }
</script>

<div class="centered">
    <!-- svelte-ignore a11y-click-events-have-key-events a11y-no-noninteractive-element-interactions -->
    <dialog bind:this={dialog} on:click|self={() => dialog.close()}>
        <!-- svelte-ignore a11y-no-static-element-interactions -->
        <div on:click|stopPropagation>
            <h1>Ansökan mottagen</h1>
            <p>För att medlemsskapet skall godkännas så behöver medlemsavgiften på {memberPrice} kr skickas till antingen Swishnummer&nbsp;1235425467 eller Bankgironummer&nbsp;117-0224.</p>
            <p>Ange certnummer som meddelande.</p>
            <img src={"swish_" + memberPrice + ".svg"} alt="Swish QR Code" width="300px" height="300px" />
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
    <label for="pnr">Personnummer:</label>
    <input id="pnr" type="text" bind:value={pnr} placeholder="Personnummer" />
    <label for="name">Namn:</label>
    <input id="name" type="name" bind:value={name} placeholder="Namn" autocomplete="name" />
    <label for="email">Email:</label>
    <input id="email" type="email" bind:value={email} placeholder="Email" autocomplete="email" />
    <label for="tel">Telefon:</label>
    <input id="tel" type="tel" bind:value={tel} placeholder="Telefon" autocomplete="tel" />
    <label for="certlevel">Medlemsskap:</label>
    <select id="memberType" bind:value={member}>
		{#each memberType as type}
            <option id={type.value} value={type.value}>
				<label for={type.value} class="member">{type.text}</label>
			</option>
		{/each}
	</select>
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
        font-size: 1rem;
        font-family: 'Roboto', sans-serif;
    }
    .centered {
        max-width: 30rem;
        margin: 0 auto;
        border-radius: 0.5rem;
        background-color: #f2f2f2;
        padding: 1rem;
    }
    label {
        width: 100%;
        padding: 0rem 0.25rem;
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
    .level, .member {
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
        margin: 15px 0px 0px 0px;
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