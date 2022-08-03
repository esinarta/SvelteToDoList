<script lang="ts">
  let newItemTitle = '';

  let list = [
    {
      title: 'Item 1',
      done: false
    },
    {
      title: 'Item 2',
      done: false
    },
    {
      title: 'Item 3',
      done: true

    }
  ]

  const addItem = () => {
    list = [...list, {title: newItemTitle, done: false}];
    newItemTitle = '';
  }

  const removeItem = (index: number) => {
    list.splice(index, 1)
		list = list;
  }
</script>

<h1>To-Do List</h1>

<div>
  <input bind:value={newItemTitle} type="text" placeholder="New item" />
  <button on:click={addItem}>Add</button>
</div>

<div>
  {#each list as listItem, index}
    <input bind:checked={listItem.done} type="checkbox">
    <span>{listItem.title}</span>
    <button on:click={() => removeItem(index)}>Remove</button>
    <br/>
  {/each} 
</div>
