<template>
  <footer>
    <div class="overly"></div>
    <div class="container">
      <div class="content">
        <h1>Movies Statistics</h1>
        <div class="statistics">
          <p>
            <span class="count">{{ highRatedCount }}</span
            ><br />
            rate over 7
          </p>
          <p>
            <span class="count"> {{ lowRatedCount }} </span><br />
            rate less 5
          </p>
          <p>
            <span class="count"
              >{{ totalVotes.firstTwoNumbers }}{{ totalVotes.count }}</span
            ><br />
            votes count
          </p>
          <p>
            <span class="count">{{ actionPercent }}%</span><br />
            Action Titles
          </p>
        </div>
      </div>
    </div>
  </footer>
</template>

<script>
export default {
  name: "FooterBar",
  props: {
    highRatedCount: {
      type: Number,
      required: true,
    },
    lowRatedCount: {
      type: Number,
      required: true,
    },
    totalVotes: {
      type: Number,
      required: true,
    },
    actionPercent: {
      type: Number,
      required: true,
    },
  },
};
</script>

<style scoped>
footer .content {
  width: 550px;
}
footer .content h1 {
  margin-bottom: 25px;
}
footer .statistics {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  width: 100%;
  gap: 20px;
}

footer .statistics p {
  color: #e0e0e0;
  font-size: 12px;
  background-color: rgba(0, 0, 0, 0.7);
  padding: 10px;
  border-radius: 6px;
  text-transform: capitalize;
  width: calc(100% / 4 - 20px);
  text-transform: capitalize;
}

footer .statistics p > span.count {
  font-size: 30px;
}
@media only screen and (max-width: 575.98px) {
  footer .content {
    width: 100%;
  }
  footer .statistics p {
    width: calc(100% / 2 - 10px);
    padding: 5px;
  }
  footer .statistics p > span {
    font-size: 14px;
  }
}
</style>
