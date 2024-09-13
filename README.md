# typing-script-for-TypingClub
A JavaScript automation script for TypingClub that automatically simulates typing with random fast and slow keystrokes


const minDelay = 100, maxDelay = 400, keyOverrides = { [String.fromCharCode(160)]: ' ' };

const getTargetCharacters = () => Array.from(document.querySelectorAll('.token span.token_unit'))
  .map(el => el.firstChild?.classList?.contains('_enter') ? '\n' : el.textContent[0])
  .map(c => keyOverrides[c] || c);

const recordKey = chr => window.core.record_keydown_time(chr);

const sleep = ms => new Promise(r => setTimeout(r, ms));

async function autoPlay(finish) {
  const chrs = getTargetCharacters();
  for (let i = 0; i < chrs.length - (!finish); i++) {
    recordKey(chrs[i]);
    await sleep(Math.random() * (maxDelay - minDelay) + minDelay);
  }
}

autoPlay(true);
