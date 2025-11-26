<template>
  <div>
    {{ burger.name }} {{ burger.kCal }}
  </div>

  <div class="burgers">
             <div class="chiliburger">
                <h3>Chiliburgare</h3>
                <img src="/img/chiliburger.jpg" alt="Chiliburgare" title="Our spicy chiliburger">
            <section class="allergies">
                <p> Allergies <span id="chiliburger-allergies">
                    <ul>
                        <li>Tomato</li>
                        <li>Lactose</li>
                        <li>Gluten</li>
                    </ul>
                </span>
                </p>
            </section>
            </div>
</template>

<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>