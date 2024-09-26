<script>
  import { onMount } from 'svelte';

  onMount(async () => {
  });
</script>

<div class='legend'>
  
  <ul class='key'>
    <li>Data Coverage by Administrative Level:</li>
    <li><i class='admin-icon national'>0</i> National level</li>
    <li><i class='admin-icon subnational1'>1</i> 1st subnational level</li>
    <li><i class='admin-icon subnational2'>2</i> 2nd subnational level</li>
    <li><i class='no-data'></i> No data</li>
  </ul>
</div>


<style lang='scss'>
  .legend {
    font-size: 14px;
  }
  ul {
    list-style-type: none;
    margin-top: 0;
    padding-left: 0;
  }
  li {
    display: flex;
    flex-flow: row;
    padding-bottom: 6px;
  }
  i.admin-icon {
    margin-right: 5px;
  }
</style>