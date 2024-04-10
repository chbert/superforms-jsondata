<script lang="ts">
	import SuperDebug from 'sveltekit-superforms';
	import { zodClient } from 'sveltekit-superforms/adapters';
	import { superForm } from 'sveltekit-superforms/client';
	import { formSchema } from './schema';

	export let form;

	export let className: string = '';
	export { className as class };

	const sprFrm = superForm(form, {
		dataType: 'json',
		validators: zodClient(formSchema),
		async onSubmit({ jsonData }) {
			let taintedData = { value: $formData.test.value, extraData: 'extra' };
			jsonData(taintedData);
		}
	});

	const { form: formData, enhance } = sprFrm;
</script>

<form method="POST" use:enhance>
	<input type="text" name="test" value={$formData.test} />
	<button type="submit">Submit</button>
</form>

<div class="mt-5 max-h-48 overflow-y-auto text-xs">
	<SuperDebug data={$formData} />
</div>
