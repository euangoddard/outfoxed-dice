<style>
  main {
    text-align: center;
  }

  .dice {
    display: flex;
    align-items: center;
    flex-direction: row;
    justify-content: center;
  }

  .item {
    display: inline-flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin-inline-end: 2rem;
    cursor: pointer;
  }

  .item:last-child {
    margin-inline-end: 0;
  }

  .die {
    margin: 0;
    border: 2px solid #333;
    border-radius: 4px;
    display: inline-flex;
    width: 60px;
    height: 60px;
    font-size: 3rem;
    line-height: 1;
    justify-content: center;
    align-items: center;
  }

  .paw::before {
    content: "🐾";
    display: inline-block;
  }

  .paw--double {
    font-size: 2rem;
  }

  .paw--double::after {
    content: "🐾";
  }

  .eye::before {
    content: "👁";
  }

  .held::after {
    content: "🔒";
    font-size: 2rem;
    text-align: center;
  }

  .unheld::after {
    content: "🔄";
    font-size: 2rem;
    text-align: center;
  }

  .buttons {
    padding: 2rem 0;
    font-size: 1.5rem;
  }
</style>

<script lang="ts">
  enum DieFace {
    Blank,
    Paw,
    DoublePaw,
    Eye,
  }

  interface Die {
    face: DieFace;
    held: boolean;
  }

  let dice: [Die, Die, Die] = [
    { face: DieFace.Blank, held: false },
    { face: DieFace.Blank, held: false },
    { face: DieFace.Blank, held: false },
  ];

  const rollUnheld = () => {
    dice = dice.map(rollDie);
  };

  const rollAll = () => {
    dice = dice
      .map((die) => {
        die.held = false;
        return die;
      })
      .map(rollDie);
  };

  const rollDie = (die: Die): Die => {
    if (die.held) {
      return die;
    }
    const randVal = Math.random() * 6;

    if (randVal < 2) {
      die.face = DieFace.Paw;
    } else if (randVal < 3) {
      die.face = DieFace.DoublePaw;
    } else {
      die.face = DieFace.Eye;
    }
    return die;
  };

  const getDieClass = (die: Die): string => {
    const classes = ["die"];
    switch (die.face) {
      case DieFace.Paw:
        classes.push("paw");
        break;
      case DieFace.DoublePaw:
        classes.push("paw", "paw--double");
        break;
      case DieFace.Eye:
        classes.push("eye");
        break;
    }
    return classes.join(" ");
  };

  const getHeldClass = (die: Die): string => {
    return die.held ? "held" : "unheld";
  };
</script>

<main>
  <h1>🦊 dice</h1>
  <div class="dice">
    {#each dice as die}
      <div class="item" on:click="{() => (die.held = !die.held)}">
        <figure class="{getDieClass(die)}"></figure>
        <span class="{getHeldClass(die)}"></span>
      </div>
    {/each}
  </div>
  <div class="buttons">
    <button type="button" on:click="{rollUnheld}">Roll unheld</button>
    <button type="button" on:click="{rollAll}">Roll all</button>
  </div>
</main>
