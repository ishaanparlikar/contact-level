<script>
	import '../app.css';
</script>


<main class="flex">
	<div class="py-4 px-2 border-r">
		sidebar
	</div>
	<main class="flex flex-col gap-4 w-full">
		<slot></slot>
	</main>
</main>
