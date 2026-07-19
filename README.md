# Dental YouTube Thumbnail — Nha Khoa Quốc Tế American

Tạo thumbnail YouTube chuẩn CTR cao cho kênh nha khoa, dùng gpt-image (yescale.io) qua Vercel relay để né Cloudflare 1010.

## Kết hợp 2 skill
- charlie947/youtube-thumbnail: face focal 30-50%, text 3-5 từ, 1280x720, high-contrast
- greg-prospyr/thumbcraft: AI mascot răng brand style nhất quán

## Yêu cầu
- yescale key: file local (không commit)
- relay: https://replay-proxy-three.vercel.app/api/relay
- script: từ skill yescale-image-gen (gen_image_yescale.py)

## Dùng
```
python3 gen_image_yescale.py --model gpt-image --prompt "<prompt từ SKILL.md>" --out thumb.png
```

## Brand colours
- Primary: pastel blue #A8D8EA
- Accent: pastel pink #F7B7C8 / yellow #FFD93D
