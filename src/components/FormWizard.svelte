<script>
	import {
		projectName,
		childThemeFolderName,
		webServerType,
		selectedPhpVersion,
		selectedDbVersionString,
		sshHost,
		sshUser,
		pathToWordPressOnServer
	} from '../stores/stores.js';

	import DatabaseRadioButton from './DatabaseRadioButton.svelte';

	// TODO: move them to store as well
	let phpVersions = ['8.0', '7.4', '7.3', '7.2', '7.1', '7.0', '5.6'];
	let databaseVersions = [
		{
			value: 'maria-10.3',
			displayText: 'MariaDB 10.3 (default)'
		},
		{
			value: 'maria-10.4',
			displayText: 'MariaDB 10.4'
		},
		{
			value: 'maria-10.5',
			displayText: 'MariaDB 10.5'
		},
		{
			value: 'maria-10.6',
			displayText: 'MariaDB 10.6'
		},
		{
			value: 'mysql-5.5',
			displayText: 'MySQL 5.5'
		},
		{
			value: 'mysql-5.6',
			displayText: 'MySQL 5.6'
		},
		{
			value: 'mysql-5.7',
			displayText: 'MySQL 5.7'
		},
		{
			value: 'mysql-8.0',
			displayText: 'MySQL 8.0'
		}
	];

	// filter for {#each}, thanks to https://stackoverflow.com/a/64016553
	function getDatabaseVersionsForType(databaseVersions, stringToMatch) {
		if (stringToMatch) {
			return databaseVersions.filter((version) => {
				return version.value.includes(stringToMatch);
			});
		} else {
			return databaseVersions;
		}
	}
</script>

<form>
	<div class="mb-3">
		<label class="form-label" for="projectName">Project name</label>
		<input
			class="form-control"
			id="projectName"
			type="text"
			bind:value={$projectName}
			placeholder="Project name"
		/>
	</div>
	<div class="mb-3">
		<fieldset>
			<legend>PHP version</legend>
			{#each phpVersions as phpVersion, i}
				<div class="form-check form-check-inline">
					<input
						class="form-check-input"
						id={phpVersion}
						type="radio"
						name="phpVersion"
						bind:group={$selectedPhpVersion}
						value={phpVersion}
					/>
					<label class="form-check-label" for={phpVersion}>{phpVersion}</label>
				</div>
			{/each}
		</fieldset>
	</div>

	<fieldset>
		<legend>Database version</legend>
		<!-- TODO: this is double coded currently, need to figure out how to use bind:group in nested components, see https://github.com/sveltejs/svelte/issues/2308 and then use: 
					<DatabaseRadioButton {displayText} {value} /> -->
		<div class="row">
			<div class="col">
				{#each getDatabaseVersionsForType(databaseVersions, 'maria') as { displayText, value }, i}
					<div class="form-check">
						<input
							class="form-check-input"
							id={'radioDbVersion-' + value}
							type="radio"
							name="dbVersion"
							bind:group={$selectedDbVersionString}
							{value}
						/>
						<label class="form-check-label" for={'radioDbVersion-' + value}>{displayText}</label>
					</div>
				{/each}
			</div>
			<div class="col">
				{#each getDatabaseVersionsForType(databaseVersions, 'mysql') as { displayText, value }, i}
					<!-- <DatabaseRadioButton {displayText} {value} /> -->
					<div class="form-check">
						<input
							class="form-check-input"
							id={'radioDbVersion-' + value}
							type="radio"
							name="dbVersion"
							bind:group={$selectedDbVersionString}
							{value}
						/>
						<label class="form-check-label" for={'radioDbVersion-' + value}>{displayText}</label>
					</div>
				{/each}
			</div>
		</div>
	</fieldset>

	<div class="mb-3">
		<fieldset>
			<legend>Web Server</legend>
			<div class="form-check form-check-inline">
				<input
					class="form-check-input"
					id="nginx"
					type="radio"
					name="webServerType"
					value="nginx-fpm"
					bind:group={$webServerType}
				/>
				<label class="form-check-label" for="nginx">nginx (default)</label>
			</div>
			<div class="form-check form-check-inline">
				<input
					class="form-check-input"
					id="apache2"
					type="radio"
					value="apache-fpm"
					name="webServerType"
					bind:group={$webServerType}
				/>
				<label class="form-check-label" for="apache2">apache2</label>
			</div>
		</fieldset>
	</div>
	<div class="mb-3">
		<label class="form-label" for="childThemeFolderName"
			>Child theme folder (in wp-content/themes/)</label
		>
		<input
			class="form-control"
			id="childThemeFolderName"
			type="text"
			placeholder="twentytwentyone-child"
			bind:value={$childThemeFolderName}
		/>
		<div class="form-text">
			If you don't use a child theme currently, just leave this setting. Can be changed later.
		</div>
	</div>
	<div class="mb-3">
		<label class="form-label" for="sshHost">SSH host</label>
		<input
			class="form-control"
			id="sshHost"
			type="text"
			placeholder="example.org"
			bind:value={$sshHost}
		/>
	</div>
	<div class="mb-3">
		<label class="form-label" for="sshUsername">SSH username</label>
		<input
			class="form-control"
			id="sshUsername"
			type="text"
			placeholder="user123"
			bind:value={$sshUser}
		/>
	</div>
	<div class="mb-3">
		<label class="form-label" for="pathToWordPressOnServer">Path to WordPress on server</label>
		<input
			class="form-control"
			id="pathToWordPressOnServer"
			type="text"
			placeholder="/sites/my-website.eu/"
			bind:value={$pathToWordPressOnServer}
		/>
		<div class="form-text">
			Get it from <a href="https://yoast.com/wordpress-site-health/" target="_blank">Site Health</a>
			&raquo; Info &raquo; Directories & sizes &raquo; WordPress directory location
		</div>
	</div>
	<!-- 
	<div class="d-grid">
		<button class="btn btn-primary btn-lg disabled" id="submitButton" type="submit">Submit</button>
	</div>
	-->
</form>

<style lang="scss">
	input[type='radio'],
	label {
		cursor: pointer;
	}
	fieldset {
		margin-top: 5px;
	}

	legend {
		font-size: 1rem;
	}
</style>
