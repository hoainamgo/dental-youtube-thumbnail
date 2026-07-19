---
name: dental-youtube-thumbnail
description: Tạo YouTube thumbnail cho Nha Khoa Quốc Tế American bằng gpt-image (yescale.io) qua Vercel relay. Kết hợp 2 skill (charlie947/youtube-thumbnail: face focal + hook text + 1280x720; greg-prospyr/thumbcraft: AI mascot brand style). Dùng cho bài nhạc Session 1/Session 2 hoặc video nha khoa. Qua relay né Cloudflare 1010.
---

# Dental YouTube Thumbnail — Nha Khoa Quốc Tế American

## MỤC ĐÍCH
Tạo thumbnail YouTube chuẩn CTR cao cho kênh nha khoa, dùng `gpt-image` qua yescale.io + Vercel relay (né 1010). Kết hợp tinh hoa 2 skill:

**Skill A (charlie947/youtube-thumbnail):**
- Face (nha sĩ/mascot) fills 30-50% frame
- Text 3-5 từ lớn, bold, high-contrast (vàng/trắng viền đen)
- 1280x720 (16:9), 2 màu chủ đạo + 1 accent
- No watermark, no bottom-right text (chỗ icon watch)

**Skill B (greg-prospyr/thumbcraft):**
- AI mascot răng (siêu anh hùng / dễ thương)
- Brand style nhất quán (gradient hồng-cyan, logo góc)
- Prompt chi tiết, style channel

## RELAY & AUTH
- Relay: `https://replay-proxy-three.vercel.app/api/relay` (edge runtime)
- Yescale: `Authorization: Bearer <key>` (file `/home/admin/.hermes/profiles/agent3/openai_token.txt`)
- Script: `gen_image_yescale.py` (từ skill yescale-image-gen)

## QUY TRÌNH TẠO THUMBNAIL (3 bước)
1. **Brief**: title bài hát/video + tone (tender/confident/shock) + text hook 3-5 từ
2. **Prompt**: ghép mascot + face nha sĩ + text + brand colour
3. **Gen**: `python3 gen_image_yescale.py --model gpt-image --prompt "..." --out thumb.png`

## PROMPT MẪU ĐÃ VERIFY
**Style A (nha sĩ face focal):**
> YouTube thumbnail 1280x720 16:9. A friendly dentist in white coat (face fills 40% of frame, warm smile, looking at camera) on the left. Large bold sans-serif text 'SMILE CONFIDENT' in yellow with dark outline, top-right. Pastel blue and white background, high contrast, clean modern dental clinic vibe, no watermark, no small text

**Style B (mascot brand):**
> AI-generated YouTube thumbnail for dental care channel. Cute smiling tooth mascot holding a giant toothbrush like a superhero, bright gradient background pink to cyan. Large bold sans-serif text 'HEALTHY SMILE' in white with dark outline at top-center. Bold channel logo bottom-left corner. Consistent brand style, eye-catching, professional, 16:9, no watermark, no small text

**Style C (thumbnail bài nhạc S2 #1 - Stumbling First Words):**
> YouTube thumbnail for song 'Stumbling First Words' - tender Pop-R&B love song about shy first confession. Cute smiling tooth mascot holding a folded love note, soft pastel pink and warm sunset gradient background, big space for bold title text at top, dreamy romantic music vibe, eye-catching clean modern style

## BRAND COLOURS (Nha Khoa Quốc Tế American)
- Primary: pastel blue (#A8D8EA)
- Accent: pastel pink (#F7B7C8) / yellow (#FFD93D) cho text
- Background: white / gradient hồng-xanh

## PITFALLS
- gpt-image hay sinh text bị lỗi chính tả → check kỹ, regenerate nếu cần
- size luôn 1024x1024 (yescale gpt-image không nhận 1280x720, upscale sau nếu cần)
- quality=low đủ dùng thumbnail web
- Qua relay không 1010

## KẾT QUẢ VERIFY (2026-07-20)
- thumb_skill1_youtube_style.png (1.8MB) - nha sĩ + SMILE CONFIDENT
- thumb_skill2_thumbcraft_text.png (1.6MB) - mascot + HEALTHY SMILE
- thumb_s2_01_stumbling_first_words.png (1.5MB) - bài hát S2#1
- Đều qua gpt-image + relay
