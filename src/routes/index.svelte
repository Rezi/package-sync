<script lang="ts">
	export let toPackage = ``;
	export let fromPackage = ``;
	export let newPackage;

	function doSync() {
		try {
			const fromJP = JSON.parse(fromPackage);
			const toJP = JSON.parse(toPackage);
			const targetJP = JSON.parse(toPackage);

			const allFromDependencies = { ...fromJP.dependencies, ...fromJP.devDependencies };

			for (let toDepKey in toJP.dependencies) {
				if (allFromDependencies.hasOwnProperty(toDepKey)) {
					targetJP.dependencies[toDepKey] = allFromDependencies[toDepKey];
				}
			}
			for (let toDevDepKey in toJP.devDependencies) {
				if (allFromDependencies.hasOwnProperty(toDevDepKey)) {
					targetJP.devDependencies[toDevDepKey] = allFromDependencies[toDevDepKey];
				}
			}

			newPackage = JSON.stringify(targetJP, null, 2);
		} catch (e) {
			console.error(e);
		}
	}
</script>

<div class="wrap">
	<div class="textareas">
		<div class="textarea-wrap">
			Web Smart Json
			<textarea bind:value={fromPackage} />
		</div>
		<div class="textarea-wrap">
			Library to Update Json
			<textarea bind:value={toPackage} />
		</div>
		<div class="textarea-wrap">
			New Library Json
			<textarea bind:value={newPackage} />
		</div>
	</div>

	<div class="buttons">
		<button on:click={doSync}>Sync</button>
	</div>
</div>

<style>
	.wrap {
		height: 100vh;
		width: 100vw;
		display: flex;
		flex-direction: column;
		align-items: stretch;
		overflow: hidden;
	}

	.textareas {
		display: flex;
		flex-grow: 1;
		width: 100vw;
	}

	.textarea-wrap {
		width: 100%;
		display: flex;
		flex-direction: column;
	}

	textarea {
		width: 100%;
		height: 100%;
	}

	.buttons {
		height: 50px;
	}
</style>
