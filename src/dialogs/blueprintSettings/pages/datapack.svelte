<script lang="ts">
	import { onDestroy } from 'svelte'
	import {
		validateDataPackFolder,
		validateJsonPath,
		validateZipPath,
	} from '../../../formats/blueprint/settings'
	import BoxSelect from '../../../svelteComponents/sidebarDialogItems/boxSelect.svelte'
	import Checkbox from '../../../svelteComponents/sidebarDialogItems/checkbox.svelte'
	import SelectFile from '../../../svelteComponents/sidebarDialogItems/selectFile.svelte'
	import SelectFolder from '../../../svelteComponents/sidebarDialogItems/selectFolder.svelte'
	import { createScopedTranslator } from '../../../util/lang'

	const translate = createScopedTranslator('dialog.blueprint_settings')

	let dataPackExportFormat = $state(Project.animated_java.data_pack_export_mode)
	let dataPackLocation = $state(Project.animated_java.data_pack)
	let jsonFile = $state(Project.animated_java.json_file)
	let bakedAnimations = $state(Project.animated_java.baked_animations)
	let animationSystem = $state(
		Project.animated_java.use_storage_for_animation ? 'storage' : 'functions'
	)
	let autoUpdateRigOrientation = $state(Project.animated_java.auto_update_rig_orientation)

	onDestroy(() => {
		Project.animated_java.data_pack_export_mode = dataPackExportFormat
		Project.animated_java.data_pack = dataPackLocation
		Project.animated_java.json_file = jsonFile
		Project.animated_java.baked_animations = bakedAnimations
		Project.animated_java.use_storage_for_animation = animationSystem === 'storage'
		Project.animated_java.auto_update_rig_orientation = autoUpdateRigOrientation
	})
</script>

<div class="dialog-page-container">
	<BoxSelect
		label={translate('data_pack_export_mode.title')}
		options={{
			folder: {
				type: 'text',
				label: translate('data_pack_export_mode.options.folder.title'),
				description: translate('data_pack_export_mode.options.folder.description'),
			},
			zip: {
				type: 'text',
				label: translate('data_pack_export_mode.options.zip.title'),
				description: translate('data_pack_export_mode.options.zip.description'),
			},
			json: {
				type: 'text',
				label: translate('data_pack_export_mode.options.json.title'),
				description: translate('data_pack_export_mode.options.json.description'),
			},
			none: {
				type: 'text',
				label: translate('data_pack_export_mode.options.none.title'),
				description: translate('data_pack_export_mode.options.none.description'),
			},
		}}
		bind:selected={dataPackExportFormat}
	></BoxSelect>

	{#if dataPackExportFormat === 'folder'}
		<SelectFolder
			label={translate('data_pack.folder.title')}
			description={translate('data_pack.folder.description')}
			bind:value={dataPackLocation}
			checkValue={validateDataPackFolder}
			required
		></SelectFolder>
	{:else if dataPackExportFormat === 'zip'}
		<SelectFile
			label={translate('data_pack.zip.title')}
			description={translate('data_pack.zip.description')}
			bind:value={dataPackLocation}
			checkValue={validateZipPath}
			required
		></SelectFile>
	{:else if dataPackExportFormat === 'json'}
		<SelectFile
			label={translate('json_file.title')}
			description={translate('json_file.description')}
			extensions={['.json']}
			bind:value={jsonFile}
			checkValue={validateJsonPath}
			required
		></SelectFile>
	{/if}

	{#if dataPackExportFormat === 'json'}
		<Checkbox
			label={translate('baked_animations.title')}
			description={translate('baked_animations.description')}
			bind:value={bakedAnimations}
		></Checkbox>
	{:else if dataPackExportFormat !== 'none'}
		<Checkbox
			label={translate('auto_update_rig_orientation.title')}
			description={translate('auto_update_rig_orientation.description')}
			bind:value={autoUpdateRigOrientation}
		></Checkbox>

		<BoxSelect
			label={translate('animation_system.title')}
			description={translate('animation_system.description')}
			options={{
				functions: {
					type: 'text',
					label: translate('animation_system.options.functions.label'),
					description: translate('animation_system.options.functions.description'),
				},
				storage: {
					type: 'text',
					label: translate('animation_system.options.storage.label'),
					description: translate('animation_system.options.storage.description'),
				},
			}}
			bind:selected={animationSystem}
		></BoxSelect>
	{/if}
</div>

<style>
	.dialog-page-container {
		overflow-y: auto;
		max-height: 75vh;
		padding-right: 16px;
		padding-left: 2px;
	}
</style>
