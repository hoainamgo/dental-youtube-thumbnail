---
name: dental-youtube-thumbnail
description: Tạo YouTube thumbnail cho Nha Khoa Quốc Tế American (brand răng mascot). Dùng Flux-2-Klein-4B (302.ai $0.014) qua Vercel relay làm chính, yescale gpt-image fallback. Typography tầng lớp (badge + headline + secondary + hook), 8 phong cách đồng bộ chuẩn music. Qua relay né Cloudflare 1010.
---

# Dental YouTube Thumbnail — Nha Khoa Quốc Tế American

## MỤC ĐÍCH
Tạo thumbnail YouTube chuẩn CTR cao cho kênh nha khoa, brand **răng mascot** nhất quán.
- **Model chính**: Flux-2-Klein-4B (302.ai, $0.014) qua relay — rẻ, nhanh, support 1280x720 ngang.
- **Fallback**: yescale gpt-image (khi Flux lỗi / cần text sắc nét hơn).
- Kết hợp: charlie947 (face/mascot 30-50%, text) + greg-prospyr (mascot brand) + layered typography chuẩn music skill.

## RELAY & AUTH
- Relay: `https://replay-proxy-three.vercel.app/api/relay` (edge runtime)
- 302.ai key: `/home/admin/.hermes/profiles/agent3/302_key.txt` (Flux chính)
- Yescale key: `/home/admin/.hermes/profiles/agent3/yescale_token.txt` (fallback)
- Script: `gen_image_302.py` (Flux) + `gen_image_yescale.py` (yescale) — từ skill yescale-image-gen

## 🔴 RULE BẮT BUỘC (user reject 2 lần thiếu text / sai tỉ lệ)
- PHẢI CÓ **4 TEXT BLOCKS** (badge + 2 headline + hook) hiển thị rõ, đọc được ở 320px.
- PHẢI TỈ LỆ **1280x720 NGANG (16:9)** — user reject bản vuông.
- Prompt LUÔN có: `16:9` / `1280x720 horizontal` + `left half layered typography` + text rõ.
- Sau gen: MỞ ẢNH CHECK (a) đủ 4 text, (b) đúng ngang → regenerate nếu thiếu/sai.

## QUY TRÌNH (4 bước)
1. **Brief**: chủ đề (trồng răng / niềng / tẩy trắng...) + tone + 4 text (badge/primary/secondary/hook)
2. **Brand**: mascot răng + gradient hồng-cyan + logo góc
3. **Build prompt** (template bên dưới, THÊM `1280x720 horizontal` + layered text)
4. **Gen**: `python3 gen_image_302.py --model flux4b --size 1280x720 --prompt "..." --out thumb_dental.png`

## 🎯 TYPOGRAPHY TẦNG LỚP (đồng bộ chuẩn music)
1. **BADGE**: nhãn nhỏ (vd "NEW SMILE" / "TRUSTED CLINIC") — deep color, sparkle, thin outline
2. **PRIMARY HEADLINE**: chữ lớn, ivory/white, outline đậm, drop shadow
3. **SECONDARY HEADLINE**: chữ LỚN NHẤT, accent (pink/yellow), hơi nghiêng, glossy
4. **HOOK BURST**: text hook nhỏ trong heart/starburst, deep color

## 🎯 FLUX PROMPTING GUIDE
- ❌ KHÔNG negative prompt (Flux phạt ngược)
- ✅ Flowing prose: subject → setting → details → lighting → mood
- ✅ Hex color codes (#A8D8EA pastel blue, #F7B7C8 pink, #FFD93D yellow)
- ✅ Cinematic keywords: "volumetric backlight", "anamorphic flare", "shallow depth of field", "8k detail, film grain"
- ✅ Luôn `16:9` / `1280x720 horizontal` + layered text left half

## 8 PHONG CÁCH (đồng bộ chuẩn music skill)
- **A. Clinic Confident (Neo-Noir tone)**: nha sĩ face + mascot, pastel blue neon, badge NEW SMILE / primary SMILE / secondary CONFIDENT / hook TODAY?
- **B. Mascot Hero (Golden)**: răng mascot siêu anh hùng, gradient hồng-cyan, badge HERO SMILE / primary BRIGHT / secondary SMILE / hook FREE?
- **C. Healing Pink (Chữa lành)**: viền trái tim, ngôi sao mờ ảo, gam hồng ấm — chủ đề êm dịu (chăm sóc bé, tẩy trắng nhẹ). badge SOFT CARE / primary GENTLE / secondary SMILE / hook HEALING?
- **D. Vintage Trust**: 1970s tungsten, badge FAMILY CLINIC / primary 20 YEARS / secondary TRUSTED / hook SINCE?
- **E. Epic Cinemascope**: mascot bay city dusk, badge BIG SMILE / primary WE / secondary SHINE / hook FOR YOU?
- **F. Premium Romance (mascot version)**: mascot bên phải, JP typography burgundy/ivory, badge NEW SMILE / primary I CAN / secondary SMILE / hook CONFIDENT?
- **G. Warm Acoustic (mascot cozy)**: mascot bên cửa sổ ấm, indie poster, badge WARM CARE / primary SMILE / secondary BRIGHTER / hook [3-block]
- **H. Blade Runner (neon clinic)**: mascot neon sci-fi, badge NEON CARE / primary WHITE / secondary TEETH / hook GLOW?

### Prompt mẫu (Style B — Mascot Hero, copy đổi text):
```text
Create a cinematic YouTube thumbnail 16:9 for dental clinic. right half: a cute smiling tooth mascot holding a giant toothbrush like a superhero, bright gradient background pink #F7B7C8 to cyan #A8D8EA, glossy highlights, bokeh, shallow depth, 8k, film grain. left half layered typography: 1. TOP BADGE 'HERO SMILE' small deep-cyan rounded label gold sparkle bold uppercase white thin outline. 2. PRIMARY 'BRIGHT' large upper-left white #FFFFFF thick white outline #1A3A5A inner outline dark shadow. 3. SECONDARY 'SMILE' below much larger angled #FFD93D thick ivory outline deep shadow glossy, largest. 4. LOWER HOOK soft starburst #F7B7C8 outline 'FREE?' deep pink 'FREE?' larger. high contrast, no extra text, no watermark, clean brand style.
```

### Prompt mẫu (Style C — Healing Pink, copy đổi text):
```text
Create a cinematic YouTube thumbnail 16:9 for dental care. right half: a cute tooth mascot wrapped in a soft glowing heart-shaped frame border, pastel pink #F8BBD0 and warm rose #F48FB1 palette, gentle floating stars and soft bokeh light orbs, dreamy low-contrast glow, volumetric soft pink backlight, shallow depth, warm loving healing mood, glossy, 8k, film grain. left half layered typography: 1. TOP BADGE 'SOFT CARE' small deep-rose rounded label white sparkle bold uppercase #FFF0F5 thin outline. 2. PRIMARY 'GENTLE' large upper-left #FFF0F5 thick white outline #C2185B inner outline dark shadow. 3. SECONDARY 'SMILE' below much larger angled #F48FB1 thick ivory outline deep shadow glossy, largest. 4. LOWER HOOK soft heart burst #F8BBD0 outline 'HEALING?' deep rose 'HEALING?' larger. high contrast, no extra text, no watermark.
```

## BRAND COLOURS (Nha Khoa Quốc Tế American)
- Primary: pastel blue (#A8D8EA)
- Accent: pastel pink (#F7B7C8) / yellow (#FFD93D) cho text
- Background: white / gradient hồng-xanh

## FALLBACK YESCALE (khi Flux lỗi)
```bash
python3 gen_image_yescale.py --model gpt-image --prompt "..." --out thumb_dental.png
```
> yescale size 1024x1024 (không nhận 1280x720) → upscale sau. Hay gặp 403 quota → quay lại Flux.

## PITFALLS
- Flux hay bỏ/sai text → check kỹ, regenerate.
- Luôn `--size 1280x720` cho Flux.
- BATCH: dùng `terminal(background=true)` (KHÔNG nohup/setsid).
- ⚠️ 302.ai: CHỈ Flux-2-Klein-4B chạy được (GLM/Kling cần enable).
- ⚠️ yescale 403 = quota/key sai (KHÔNG phải 1010). Hết quota → Flux.

## KẾT QUẢ VERIFY (2026-07-20)
- thumb_skill1_youtube_style.png (1.8MB) - nha sĩ + SMILE CONFIDENT (yescale)
- thumb_skill2_thumbcraft_text.png (1.6MB) - mascot + HEALTHY SMILE (yescale)
- Đã nâng cấp: Flux chính + 1280x720 + layered typography (4 blocks) + Healing Pink + 8 styles đồng bộ music
