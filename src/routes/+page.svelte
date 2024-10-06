<script lang="ts">
	import Bar from '$lib/components/Bar.svelte';
	import * as Card from '$lib/components/ui/card';
	import CalendarIcon from 'lucide-svelte/icons/calendar';
	import type { DateRange } from 'bits-ui';
	import {
		CalendarDate,
		DateFormatter,
		type DateValue,
		getLocalTimeZone,
		today
	} from '@internationalized/date';
	import { cn } from '$lib/utils.js';
	import { Button } from '$lib/components/ui/button/index.js';
	import { RangeCalendar } from '$lib/components/ui/range-calendar/index.js';
	import * as Popover from '$lib/components/ui/popover/index.js';
	import * as ToggleGroup from '$lib/components/ui/toggle-group/index.js';

	const df = new DateFormatter('en-US', {
		dateStyle: 'medium'
	});

	let value: DateRange | undefined = {
		start: new CalendarDate(2022, 1, 20),
		end: new CalendarDate(2022, 1, 20).add({ days: 20 })
	};

	let startValue: DateValue | undefined = undefined;

	const start = today(getLocalTimeZone());
	const end = start.add({ days: 7 });
</script>

<section class="flex flex-col gap-4 p-4">
	<div class="flex justify-between">
		<div class="flex gap-2">
			<Popover.Root openFocus>
				<Popover.Trigger asChild let:builder>
					<Button
						variant="outline"
						class={cn(
							'w-[300px] justify-start text-left font-normal',
							!value && 'text-muted-foreground'
						)}
						builders={[builder]}
					>
						<CalendarIcon class="mr-2 h-4 w-4" />
						{#if value && value.start}
							{#if value.end}
								{df.format(value.start.toDate(getLocalTimeZone()))} - {df.format(
									value.end.toDate(getLocalTimeZone())
								)}
							{:else}
								{df.format(value.start.toDate(getLocalTimeZone()))}
							{/if}
						{:else if startValue}
							{df.format(startValue.toDate(getLocalTimeZone()))}
						{:else}
							Pick a date
						{/if}
					</Button>
				</Popover.Trigger>
				<Popover.Content class="w-auto p-0" align="start">
					<RangeCalendar
						bind:value
						bind:startValue
						initialFocus
						numberOfMonths={2}
						placeholder={value?.start}
					/>
				</Popover.Content>
			</Popover.Root>
			<div class="flex rounded">
				<ToggleGroup.Root type="single" size="sm" class="rounded-lg bg-muted p-1">
					<ToggleGroup.Item class="h-6 text-xs data-[state=on]:bg-white" value="0"
						>Today</ToggleGroup.Item
					>
					<ToggleGroup.Item class="h-6 text-xs data-[state=on]:bg-white" value="1w"
						>1w</ToggleGroup.Item
					>
					<ToggleGroup.Item class="h-6 text-xs data-[state=on]:bg-white" value="1m"
						>1m</ToggleGroup.Item
					>
					<ToggleGroup.Item class="h-6 text-xs data-[state=on]:bg-white" value="3m"
						>3m</ToggleGroup.Item
					>
					<ToggleGroup.Item class="h-6 text-xs data-[state=on]:bg-white" value="6m"
						>6m</ToggleGroup.Item
					>
					<ToggleGroup.Item class="h-6 text-xs data-[state=on]:bg-white" value="1y"
						>1y</ToggleGroup.Item
					>
				</ToggleGroup.Root>
			</div>
		</div>
		<Button>Upload Contacts</Button>
	</div>
	<section class="flex w-full justify-between gap-4">
		<Card.Root class="max-w-2xl basis-2/5">
			<Card.Header>
				<Card.Title>Card Title</Card.Title>
			</Card.Header>
			<Card.Content>
				<Bar />
			</Card.Content>
		</Card.Root>
		<div class="grid h-fit basis-3/5 grid-cols-3 gap-4">
			{#each [...Array(6).keys()] as x}
				<Card.Root class="h-36 w-full">
					<Card.Header>
						<Card.Title>Card Title</Card.Title>
					</Card.Header>
					<Card.Content>
						{x}
					</Card.Content>
				</Card.Root>
			{/each}
		</div>
	</section>
</section>
