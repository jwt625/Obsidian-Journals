
2025-08-31T02:43:54-07:00
Vibe coded most of FE, and need to start working on github oauth:
![[Pasted image 20250831024414.png]]

2025-08-31T03:18:29-07:00
Auth ran thru:
- still need to implement the actual conversion and config
![[Pasted image 20250831031832.png]]

2025-08-31T03:29:23-07:00
hmm why is the text recognition sooo slow??
![[Screenshot 2025-08-31 at 03.30.15.png]]

```
Creating model dict...
2025-08-31 03:27:09,211 [WARNING] surya: `TableRecEncoderDecoderModel` is not compatible with mps backend. Defaulting to cpu instead
Creating PdfConverter...
Converting /Users/wentaojiang/Documents/GitHub/PlayGround/20250831_one_click_paper_page/tests/pdf/2508.19977v1.pdf...
Recognizing layout: 100%|███████████████████████████████████████████| 3/3 [00:08<00:00,  2.86s/it]
Running OCR Error Detection: 100%|██████████████████████████████████| 5/5 [00:01<00:00,  4.61it/s]
Detecting bboxes: 100%|█████████████████████████████████████████████| 3/3 [00:02<00:00,  1.19it/s]
Recognizing Text:  50%|█████████████████████▏                    | 61/121 [03:18<02:57,  2.96s/it]
```


2025-08-31T03:34:08-07:00
![[Screenshot 2025-08-31 at 03.36.00.png]]
- what is wrong with python??
- ohh it is the camera stream from the bay bridge traffic cam project.

2025-08-31T12:40:21-07:00
Conversion is going thru from FE to BE and back:
![[Pasted image 20250831124032.png]]












