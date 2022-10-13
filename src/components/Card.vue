<template>
    <div class="card">

        <div class="delete">
          <slot name="delete"/>
        </div>

        <div class="update">
          <slot name="update"/>
        </div>

        <div class="title">
            <slot name="title"/>
        </div>


        <div class="content">
            <slot name="content"/>
        </div>

        <div class="description">
            <slot name="description"/>
        </div>
    </div>
</template>


<script lang="ts">
export default{

}
</script>

<style scoped >

.card {
  border: 1px solid silver;
  border-radius: 8px;
  max-width: 200px;
  margin: 0 5px;
  cursor: pointer;
  box-shadow: 0px 1px 3px darkgrey;
  transition: 0.2s;
}
.title, .content, .description,.delete {
  padding: 16px;
  text-transform: capitalize;
  text-align: center;
}
.title, .content {
  border-bottom: 1px solid silver;
}
.title {
  font-size: 1.25em;
}
.card:hover {
  transition: 0.2s;
  box-shadow: 0px 1px 9px darkgrey;
}

</style>