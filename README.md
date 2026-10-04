/**
 * ═══════════════════════════════════════════════════════════════════
 *  ⚡ BL@CKN1TE-MD — NEON LIVE README GENERATOR (v2.0)
 *  🛡️ Powered by BL@CKN1TE — THE INCISIVE TRI-HAT HACKER
 *  One file. Full neon. Live telemetry. Auto-writes README.md
 * ═══════════════════════════════════════════════════════════════════
 */

import * as fs from 'fs';
import * as path from 'path';
import axios from 'axios';

// ─────────────────────────── CONFIG ───────────────────────────
const CONFIG = {
  username:   process.env.USERNAME   || 'Blackn1te-crypto',
  repo:       process.env.REPO       || 'BLACKN1TE-MD-BOT',
  token:      process.env.GITHUB_TOKEN || '',
  tgBot:      'BLCKN1TE_MD_BOT',
  tgLink:     'https://t.me/BLCKN1TE_MD_BOT',
  tgContact:  'https://t.me/blackn1te_incisive_trihathacker',
  outFile:    path.resolve(process.cwd(), 'README.md'),
};

// ─────────────────────────── NEON ANSI ───────────────────────────
const N = {
  green:  (s: string) => `\x1b[92m${s}\x1b[0m`,
  cyan:   (s: string) => `\x1b[96m${s}\x1b[0m`,
  purple: (s: string) => `\x1b[95m${s}\x1b[0m`,
  pink:   (s: string) => `\x1b[38;5;206m${s}\x1b[0m`,
  dim:    (s: string) => `\x1b[2m${s}\x1b[0m`,
  bold:   (s: string) => `\x1b[1m${s}\x1b[0m`,
};

// ─────────────────────────── TYPES ───────────────────────────
interface LiveData {
  stars: number;
  forks: number;
  followers: number;
  repos: number;
  watchers: number;
  issues: number;
  lastPing: string;
  generatedAt: string;
}

// ─────────────────────────── FETCH LIVE ───────────────────────────
async function fetchLive(): Promise<LiveData> {
  const headers = CONFIG.token ? { Authorization: `token ${CONFIG.token}` } : {};
  const now = new Date().toUTCString();

  try {
    const [user, repo] = await Promise.all([
      axios.get(`https://api.github.com/users/${CONFIG.username}`, { headers }),
      axios.get(`https://api.github.com/repos/${CONFIG.username}/${CONFIG.repo}`, { headers }),
    ]);

    return {
      stars:       repo.data.stargazers_count   ?? 0,
      forks:       repo.data.forks_count        ?? 0,
      followers:   user.data.followers          ?? 0,
      repos:       user.data.public_repos       ?? 0,
      watchers:    repo.data.subscribers_count  ?? 0,
      issues:      repo.data.open_issues_count  ?? 0,
      lastPing:    new Date(repo.data.pushed_at).toUTCString(),
      generatedAt: now,
    };
  } catch (err) {
    console.error(N.purple('⚠️  Live fetch failed — using fallback:'), (err as Error).message);
    return {
      stars: 0, forks: 0, followers: 0, repos: 0, watchers: 0, issues: 0,
      lastPing: 'unknown', generatedAt: now,
    };
  }
}

// ─────────────────────────── BUILD README ───────────────────────────
function buildReadme(live: LiveData): string {
  return `<div align="center">

<!-- ══════════════ NEON WAVE HEADER ══════════════ -->
<img src="https://capsule-render.vercel.app/api?type=waving&color=0:00FFAA,25:00BFFF,50:BF00FF,75:FF00AA,100:00FFAA&height=230&section=header&text=BL%40CKN1TE-MD&fontSize=74&fontColor=ffffff&animation=twinkling&fontAlignY=32&desc=POWERED%20BY%20THE%20INCISIVE%20TRI-HAT%20HACKER&descAlignY=56&descSize=16" width="100%"/>

<!-- ══════════════ NEON TYPING CAPTIONS ══════════════ -->
<a href="https://github.com/${CONFIG.username}">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=800&size=24&duration=2200&pause=500&color=00FFAA&center=true&vCenter=true&multiline=true&width=820&height=150&lines=%3E_+const+bot+%3D+new+BLACKN1TE_MD()%3B;%3E_+bot.engine+%3D+%22TypeScript%22%3B;%3E_+bot.sync(%22Telegram%22)%3B+%2F%2F+%40${CONFIG.tgBot};%3E_+bot.deploy()%3B+%2F%2F+%F0%9F%9B%A1%EF%B8%8F+TRI-HAT+ONLINE" alt="Neon TS Captions"/>
</a>

<br/>

<a href="${CONFIG.tgLink}">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=700&size=18&duration=3000&pause=800&color=BF00FF&center=true&vCenter=true&width=760&height=60&lines=%F0%9F%94%97+%40${CONFIG.tgBot}+%E2%80%A2+SYNCED+%26+LIVE+24%2F7;%E2%9A%A1+Powered+by+BL%40CKN1TE+THE+INCISIVE+TRI-HAT+HACKER" alt="Neon Tagline"/>
</a>

</div>

---

<div align="center">

### ⚡ 𝗟𝗜𝗩𝗘 𝗠𝗢𝗡𝗜𝗧𝗢𝗥 — 𝗥𝗘𝗔𝗟-𝗧𝗜𝗠𝗘 𝗧𝗘𝗟𝗘𝗠𝗘𝗧𝗥𝗬

\`\`\`ts
// ⚡ NEON STATUS — live
const status = {
  core:      "🟢 ONLINE",
  engine:    "TypeScript",
  telegram:  "@${CONFIG.tgBot}",
  lastPing:  "${live.lastPing}",
  triHat:    "🛡️ ACTIVE",
  generated: "${live.generatedAt}"
};
\`\`\`

\`\`\`ts
// 📊 GITHUB TELEMETRY — live
const telemetry = {
  stars:     "${live.stars}",
  forks:     "${live.forks}",
  followers: "${live.followers}",
  repos:     "${live.repos}",
  watchers:  "${live.watchers}",
  issues:    "${live.issues}"
};
\`\`\`

\`\`\`ts
// 🔗 TELEGRAM BRIDGE — live
const telegramBridge = {
  bot:     "@${CONFIG.tgBot}",
  link:    "${CONFIG.tgLink}",
  contact: "${CONFIG.tgContact}",
  status:  "⚡ CONNECTED"
};
\`\`\`

</div>

---

## 🤖 𝗟𝗜𝗩𝗘 𝗕𝗢𝗧 𝗟𝗜𝗡𝗞

<div align="center">

<a href="${CONFIG.tgLink}">
  <img src="https://img.shields.io/badge/⚡_OPEN_@BLCKN1TE__MD__BOT-00FFAA?style=for-the-badge&labelColor=000000&logo=telegram&logoColor=00FFAA" height="44"/>
</a>

<br/><br/>

> **🔗 [${CONFIG.tgLink}](${CONFIG.tgLink})**

</div>

---

## 🛠️ 𝗜𝗡𝗦𝗧𝗔𝗟𝗟𝗔𝗧𝗜𝗢𝗡 — 𝗧𝗘𝗟𝗘𝗚𝗥𝗔𝗠 𝗦𝗬𝗡𝗖 𝗠𝗘𝗧𝗛𝗢𝗗

> **No QR panic. No terminal chaos.** Create a Telegram bot, sync it, and both platforms go live in one shot.

\`\`\`ts
// ═══════════════ INSTALL SEQUENCE ═══════════════
const install = async () => {
  await git.clone("https://github.com/${CONFIG.username}/${CONFIG.repo}.git");
  await npm.install();

  const telegramBot = await botFather.createBot({
    name:     "BLCKN1TE_MD",
    username: "@${CONFIG.tgBot}"
  });

  await bot.sync({
    telegram: telegramBot.token,
    whatsapp: "pairing-code",
    mode:     "multi-device"
  });

  await bot.launch(); // 🛡️ TRI-HAT ONLINE
};
\`\`\`

### 🔥 Steps

**1️⃣ Clone + Install**
\`\`\`bash
git clone https://github.com/${CONFIG.username}/${CONFIG.repo}.git
cd ${CONFIG.repo}
npm install
\`\`\`

**2️⃣ Create Telegram Bot**
- Open Telegram → search **@BotFather**
- Send \`/newbot\` → name it → username: **@${CONFIG.tgBot}**
- Copy the **BOT TOKEN**

**3️⃣ Sync WhatsApp + Telegram**
\`\`\`bash
npm run link -- --token=YOUR_TELEGRAM_BOT_TOKEN
\`\`\`
- Bot sends **pairing code** via [@${CONFIG.tgBot}](${CONFIG.tgLink})
- Paste code → WhatsApp → **Linked Devices**
- ✅ **Sync complete**

**4️⃣ Launch**
\`\`\`bash
npm start
\`\`\`

---

## ✨ 𝗙𝗘𝗔𝗧𝗨𝗥𝗘𝗦

<div align="center">

| ⚡ Neon Feature | 🟢 Status | 🛡️ Shield |
|---|---|---|
| Multi-Device WhatsApp | \`ONLINE\` | 🟢 |
| Telegram Bridge Sync | \`ONLINE\` | 🟢 |
| Group Management | \`ONLINE\` | 🟢 |
| Media Downloader | \`ONLINE\` | 🟢 |
| AI Auto-Reply | \`ONLINE\` | 🟢 |
| Anti-Link / Anti-Spam | \`ONLINE\` | 🟢 |

</div>

---

## 🎨 𝗧𝗘𝗖𝗛 𝗦𝗧𝗔𝗖𝗞

<div align="center">

![TypeScript](https://img.shields.io/badge/TypeScript-00FFAA?style=for-the-badge&logo=typescript&logoColor=black)
![Node.js](https://img.shields.io/badge/Node.js-00BFFF?style=for-the-badge&logo=node.js&logoColor=black)
![WhatsApp](https://img.shields.io/badge/WhatsApp-BF00FF?style=for-the-badge&logo=whatsapp&logoColor=white)
![Telegram](https://img.shields.io/badge/Telegram-00FFAA?style=for-the-badge&logo=telegram&logoColor=black)

</div>

---

## 👨‍💻 𝗔𝗨𝗧𝗛𝗢𝗥

<div align="center">

### 🛡️ 𝗕𝗟@𝗖𝗞𝗡𝟭𝗧𝗘
**𝗧𝗛𝗘 𝗜𝗡𝗖𝗜𝗦𝗜𝗩𝗘 𝗧𝗥𝗜-𝗛𝗔𝗧 𝗛𝗔𝗖𝗞𝗘𝗥**

\`\`\`ts
// ═══════════════ OPERATOR PROFILE ═══════════════
const BLACKN1TE = {
  role:    "🛡️ Ethical Hacker",
  alias:   "The Incisive Tri-Hat Hacker",
  skills:  ["TypeScript", "Node.js", "Bot Automation", "Security"],
  bot:     "@${CONFIG.tgBot}",
  contact: "${CONFIG.tgContact}",
  github:  "https://github.com/${CONFIG.username}"
};
\`\`\`

[![GitHub](https://img.shields.io/badge/GitHub-${CONFIG.username}-00FFAA?style=for-the-badge&logo=github&logoColor=black)](https://github.com/${CONFIG.username})
[![Telegram](https://img.shields.io/badge/Telegram-@BLCKN1TE__MD__BOT-BF00FF?style=for-the-badge&logo=telegram&logoColor=white)](${CONFIG.tgLink})
[![Contact](https://img.shields.io/badge/Contact-Tri--Hat_Hacker-FF00AA?style=for-the-badge&logo=telegram&logoColor=white)](${CONFIG.tgContact})

</div>

---

<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:00FFAA,25:00BFFF,50:BF00FF,75:FF00AA,100:00FFAA&height=170&section=footer&text=POWERED+BY+BL%40CKN1TE+%E2%9A%A1+TRI-HAT+HACKER&fontSize=22&fontColor=ffffff&animation=twinkling"/>

</div>
`;
}

// ─────────────────────────── MAIN ───────────────────────────
async function main() {
  console.log(N.green('⚡ NEON README Generator v2.0 — BL@CKN1TE TRI-HAT'));
  console.log(N.dim(`   → ${CONFIG.username}/${CONFIG.repo}`));

  const live = await fetchLive();
  console.log(N.cyan(`📊 Live: ⭐${live.stars}  🍴${live.forks}  👥${live.followers}  📦${live.repos}  ⚠️${live.issues}`));

  const md = buildReadme(live);
  fs.writeFileSync(CONFIG.outFile, md, 'utf-8');

  console.log(N.purple(`✅ README.md written → ${CONFIG.outFile}`));
  console.log(N.bold(N.green('🛡️ Tri-Hat out.')));
}

main().catch((e) => {
  console.error(N.purple('💥 Fatal:'), e);
  process.exit(1);
});