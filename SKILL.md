---
name: dental-youtube-thumbnail
description: Tạo YouTube thumbnail cho Nha Khoa Quốc Tế American (brand răng mascot). Dùng Flux-2-Klein-4B (302.ai $0.014) qua Vercel relay làm chính, yescale gpt-image fallback. Bắt buộc text rõ + 1280x720 ngang. Qua relay né Cloudflare 1010.
---

# Dental YouTube Thumbnail — Nha Khoa Quốc Tế American

## MỤC ĐÍCH
Tạo thumbnail YouTube chuẩn CTR cao cho kênh nha khoa, brand **răng mascot** nhất quán.
- **Model chính**: Flux-2-Klein-4B (302.ai, $0.014) qua relay — rẻ, nhanh, support 1280x720 ngang.
- **Fallback**: yescale gpt-image (khi Flux lỗi / cần text sắc nét hơn).
- Kết hợp tinh hoa: charlie947 (face 30-50%, text 3-5 từ) + greg-prospyr (mascot brand).

## RELAY & AUTH
- Relay: `https://replay-proxy-three.vercel.app/api/relay` (edge runtime)
- 302.ai key: `/home/admin/.hermes/profiles/agent3/302_key.txt` (Flux chính)
- Yescale key: `/home/admin/.hermes/profiles/agent3/yescale_token.txt` (fallback)
- Script: `gen_image_302.py` (Flux) + `gen_image_yescale.py` (yescale) — từ skill yescale-image-gen

## 🔴 RULE BẮT BUỘC (user reject 2 lần thiếu text / sai tỉ lệ)
- PHẢI CÓ TEXT hiển thị rõ (hook 3-5 từ, bold, viền, đọc được ở 320px).
- PHẢI TỈ LỆ **1280x720 NGANG (16:9)** — user reject bản vuông.
- Prompt LUÔN có: `Large bold sans-serif text '{HOOK}' ... at top-center` + `16:9` / `1280x720 horizontal`.
- Sau gen: MỞ ẢNH CHECK (a) có text thật, (b) đúng ngang 16:9 → regenerate nếu thiếu/sai.

## QUY TRÌNH (4 bước)
1. **Brief**: chủ đề (trồng răng / niềng / tẩy trắng...) + tone (confident/tender/shock) + text hook 3-5 từ
2. **Brand**: mascot răng + gradient hồng-cyan + logo góc
3. **Build prompt** (template bên dưới, THÊM `1280x720 horizontal` + text rõ)
4. **Gen**: `python3 gen_image_302.py --model flux4b --size 1280x720 --prompt "..." --out thumb_dental.png`

## 🎯 FLUX PROMPTING GUIDE (áp dụng cho dental luôn)
- ❌ KHÔNG negative prompt (Flux phạt ngược)
- ✅ Flowing prose: subject → setting → details → lighting → mood
- ✅ Hex color codes cụ thể (#A8D8EA pastel blue, #F7B7C8 pink, #FFD93D yellow)
- ✅ Cinematic keywords: "volumetric backlight", "anamorphic flare", "shallow depth of field", "8k detail, film grain"
- ✅ Charlier947: face/mascot 30-50% frame, text 3-5 từ, 2 màu chủ đạo, high contrast

## 6 PHONG CÁCH (đồng bộ chuẩn music skill)
> Thay `{HOOK}` / `{SCENE}` cho bài nha khoa. Giữ prose + hex + cinematic.
- **A. Clinic Confident**: nha sĩ face 40% + mascot, bright white/pastel blue, "SMILE CONFIDENT"
- **B. Mascot Hero**: răng mascot siêu anh hùng, gradient hồng-cyan, "HEALTHY SMILE"
- **C. Healing Pink (Chữa lành / nhẹ nhàng)**: viền trái tim, ngôi sao mờ ảo, gam hồng ấm — dùng cho chủ đề êm dịu (vd chăm sóc bé, tẩy trắng nhẹ)
- **D. Neo-Noir**: phòng khám đêm, neon xanh, dramatic — dùng shock/before-after
- **E. Golden Trust**: golden-hour, ấm áp, gia đình — dùng chủ đề niềm tin/lâu năm
- **F. Epic Cinemascope**: răng mascot bay trên city dusk, grand scale

### Prompt mẫu (Style A — Clinic Confident, copy đổi hook):
```text
Cinematic YouTube thumbnail 16:9 for dental clinic. LARGE bold sans-serif text 'SMILE CONFIDENT' in #FFD93D with #1A3A5A outline at top-center, readable small. A friendly dentist in white coat (face fills 40% frame, warm smile) on left, a cute smiling tooth mascot on right, pastel blue #A8D8EA and white background, soft bokeh, shallow depth of field, bright clean clinic vibe, glossy highlights, 8k detail, film grain, no watermark
```

### Prompt mẫu (Style C — Healing Pink, copy đổi hook):
```text
Cinematic YouTube thumbnail 16:9 for dental care. LARGE bold rounded sans-serif text '{HOOK}' in #FFF0F5 with #C2185B outline at top-center, readable small. A cute tooth mascot wrapped in a soft glowing heart-shaped frame border, pastel pink #F8BBD0 and warm rose #F48FB1 palette, gentle floating stars and soft bokeh light orbs, dreamy low-contrast glow, volumetric soft pink backlight, shallow depth of field, warm loving healing mood, glossy soft highlights, ethereal atmosphere, 8k detail, subtle film grain, no watermark
```

## BRAND COLOURS (Nha Khoa Quốc Tế American)
- Primary: pastel blue (#A8D8EA)
- Accent: pastel pink (#F7B7C8) / yellow (#FFD93D) cho text
- Background: white / gradient hồng-xanh

## FALLBACK YESCALE (khi Flux lỗi)
```bash
python3 gen_image_yescale.py --model gpt-image --prompt "..." --out thumb_dental.png
```
> yescale size 1024x1024 (không nhận 1280x720) → upscale sau nếu cần. Hay gặp 403 quota → quay lại Flux.

## PITFALLS
- Flux hay bỏ/sai text → check kỹ, regenerate.
- Luôn `--size 1280x720` cho Flux.
- BATCH: dùng `terminal(background=true)` (KHÔNG nohup/setsid).
- ⚠️ 302.ai: CHỈ Flux-2-Klein-4B chạy được (GLM/Kling cần enable).
- ⚠️ yescale 403 = quota/key sai (KHÔNG phải 1010). Hết quota → Flux.

## KẾT QUẢ VERIFY (2026-07-20)
- thumb_skill1_youtube_style.png (1.8MB) - nha sĩ + SMILE CONFIDENT (yescale)
- thumb_skill2_thumbcraft_text.png (1.6MB) - mascot + HEALTHY SMILE (yescale)
- Đã nâng cấp chuẩn: Flux chính + 1280x720 + Healing Pink style + Flux guide
