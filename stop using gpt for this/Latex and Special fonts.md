Ah, that explains it 🙂 — a Nerd Font like **Cascadia Code NF** (or "Caskaydia Cove NF") is great for terminals, icons, and coding ligatures, but it **doesn’t include full Unicode coverage** (Chinese, Japanese, Korean, emojis, math symbols, etc.).

That’s why you’re seeing broken characters — the system has no fallback font for those ranges.

---

### ✅ What you should do

Keep your Nerd Font for coding, but also install **fallback fonts** for everything else:

```bash
sudo pacman -S noto-fonts noto-fonts-cjk noto-fonts-emoji noto-fonts-extra
```

Optional but useful:

```bash
yay -S ttf-ms-fonts ttf-unifont
```

Then rebuild font cache:

```bash
fc-cache -fv
```

---

### ⚙️ Make sure font fallback works

Browsers like Zen/Firefox will automatically pick fallback fonts if the current font doesn’t support a character. With the above installed:

- Code → will use your Nerd Font
    
- Chinese/Japanese/Korean → will fall back to `Noto Sans CJK`
    
- Emojis → will fall back to `Noto Color Emoji`
    
- Math symbols, arrows → from `Noto Extra`
    

---

### 🔍 Next step

Can you check for me:

```bash
fc-list | grep "Noto Sans CJK"
```

If that returns nothing, your system is missing the CJK fonts, which explains why Chinese isn’t rendering in your browser.

Do you want me to also show you how to **prioritize Cascadia Cove NF in terminal but still keep fallbacks system-wide** (so you don’t get ugly mixed fonts)?

