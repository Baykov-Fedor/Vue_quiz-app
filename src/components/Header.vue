<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled>
        <b>Fancy Quiz Application</b>
      </b-nav-item>
      <b-nav-item disabled>Counter: {{numCorrect}}/{{numTotal}}</b-nav-item>
      <b-nav-item disabled>Number of Question: {{numQuestion + 1}}/10</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  props: ["numTotal", "numCorrect", "numQuestion"],
};
</script>