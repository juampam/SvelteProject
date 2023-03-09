<script>
  import Button from "../atoms/Button.svelte";
  export let buttons;
</script>

<div class=" bg-neutral navbar flex start gap-1 rounded-50">
  {#each buttons as button}
    <Button
      label={button.label}
    />
  {/each}
</div>
