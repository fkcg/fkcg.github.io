<script>
    /**
	 * @type {string}
	 */
    export let id;
    /**
	 * @type {string}
	 */
    export let type;
    /**
	 * @type {string}
	 */
    export let value;
    /**
	 * @type {string}
	 */
    export let caption;
</script>

<label for={id}>{caption + ':'}</label>
<input id={id} type="text" bind:value={value} autocomplete={type == 'text' ? 'off' : type} />