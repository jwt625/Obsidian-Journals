
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


2025-09-01T00:25:54-07:00
ok we'll see about this one:
![[Pasted image 20250901002600.png]]

2025-09-01T00:30:47-07:00
Great this actually worked!
![[Pasted image 20250901003053.png]]

2025-09-01T00:39:20-07:00
let's see if this forked the template properly
![[Pasted image 20250901003928.png]]

okay, good sign:
![[Pasted image 20250901003951.png]]


2025-09-01T00:47:21-07:00
hmm
```
  
Workflows aren’t being run on this forked repository  
Because this repository contained workflow files when it was forked, we have disabled them from running on this fork. Make sure you understand the configured workflows and their expected usage before enabling Actions on this repository.
```




2025-09-01T09:33:30-07:00
Now debugging workflow, seems to be disabled on forked repo by default. Need to create a repo from fresh.
```

INFO:     127.0.0.1:62071 - "GET /api/templates HTTP/1.1" 200 OK
2025-09-01 09:32:53,397 - INFO - 🚀 Creating optimized repository: test-optimized-1756744373
2025-09-01 09:32:53,398 - INFO - 🔍 Fetching template content from academicpages/academicpages.github.io
2025-09-01 09:32:54,103 - INFO - 📋 Filtered 359 files to 271 essential files
2025-09-01 09:32:54,104 - INFO - ✅ Cached template data for academicpages/academicpages.github.io (271 files)
2025-09-01 09:32:55,606 - INFO - 📁 Copying template content to jwt625/test-optimized-1756744373
2025-09-01 09:32:56,480 - ERROR - ❌ Failed to copy template content: Failed to create tree: {'message': 'tree.sha ba132f0d204eceb3742f01f5c63e32598948f8c6 is not a valid blob', 'documentation_url': 'https://docs.github.com/rest/git/trees#create-a-tree', 'status': '422'}
2025-09-01 09:32:56,480 - ERROR - ❌ Failed to create optimized repository: Failed to create tree: {'message': 'tree.sha ba132f0d204eceb3742f01f5c63e32598948f8c6 is not a valid blob', 'documentation_url': 'https://docs.github.com/rest/git/trees#create-a-tree', 'status': '422'}
2025-09-01 09:32:56,480 - ERROR - Optimized deployment test failed: Failed to create tree: {'message': 'tree.sha ba132f0d204eceb3742f01f5c63e32598948f8c6 is not a valid blob', 'documentation_url': 'https://docs.github.com/rest/git/trees#create-a-tree', 'status': '422'}
INFO:     127.0.0.1:62071 - "POST /api/github/test-deploy-optimized HTTP/1.1" 500 Internal Server Error

```

2025-09-01T10:54:59-07:00
Let's see if this is true:
![[Pasted image 20250901105504.png]]

Ok i see the files got created, but no action it seems.
![[Pasted image 20250901105555.png]]


2025-09-01T16:10:17-07:00
```

INFO:     127.0.0.1:59489 - "GET /api/templates HTTP/1.1" 200 OK
2025-09-01 16:08:26,451 - INFO - 🏠 Creating dual deployment for: test-dual-1756768106
2025-09-01 16:08:26,451 - INFO - 🚀 Creating optimized repository: test-dual-1756768106
2025-09-01 16:08:26,451 - INFO - 🔍 Fetching template content from pages-themes/minimal
2025-09-01 16:08:26,859 - INFO - 📋 Filtered 72 files to 42 essential files
2025-09-01 16:08:26,859 - INFO - ✅ Cached template data for pages-themes/minimal (42 files)
2025-09-01 16:08:28,296 - INFO - 📁 Copying template content to jwt625/test-dual-1756768106
2025-09-01 16:08:28,681 - INFO - Creating blobs for 34 files
2025-09-01 16:08:50,693 - INFO - ✅ Template content copied successfully (34 files)
2025-09-01 16:08:50,694 - INFO - ⚙️ Adding deployment workflow to jwt625/test-dual-1756768106
2025-09-01 16:08:51,165 - WARNING - Failed to add workflow: {'message': 'Not Found', 'documentation_url': 'https://docs.github.com/rest/repos/contents#create-or-update-file-contents', 'status': '404'}
2025-09-01 16:08:51,795 - INFO - Enabled GitHub Pages with Actions for jwt625/test-dual-1756768106
2025-09-01 16:08:51,795 - INFO - ✅ Optimized repository created: jwt625/test-dual-1756768106
2025-09-01 16:08:51,795 - INFO - ✅ Dual deployment created: jwt625/test-dual-1756768106
2025-09-01 16:08:51,795 - INFO - 📍 Standalone URL: https://jwt625.github.io/test-dual-1756768106/
INFO:     127.0.0.1:59489 - "POST /api/github/test-dual-deploy HTTP/1.1" 200 OK

```
- fixing the 404 error

2025-09-01T16:28:00-07:00
Batch delete the test repo:
```
gh repo list your-username --limit 1000 --json name -q '.[].name' | grep '^test-dual-'
```

```
for repo in \
test-dual-1756769191 \
test-dual-1756768992 \
test-dual-1756768844 \
test-dual-1756768721 \
test-dual-1756768543 \
test-dual-1756768243 \
test-dual-1756768106 \
test-dual-1756767639; do
  gh repo delete your-username/$repo --confirm
done

```

Very nice:
```


Flag --confirm has been deprecated, use `--yes` instead
✓ Deleted repository jwt625/test-dual-1756769191
Flag --confirm has been deprecated, use `--yes` instead
✓ Deleted repository jwt625/test-dual-1756768992
Flag --confirm has been deprecated, use `--yes` instead
✓ Deleted repository jwt625/test-dual-1756768844
Flag --confirm has been deprecated, use `--yes` instead
✓ Deleted repository jwt625/test-dual-1756768721
Flag --confirm has been deprecated, use `--yes` instead
✓ Deleted repository jwt625/test-dual-1756768543
Flag --confirm has been deprecated, use `--yes` instead
✓ Deleted repository jwt625/test-dual-1756768243
Flag --confirm has been deprecated, use `--yes` instead
✓ Deleted repository jwt625/test-dual-1756768106
Flag --confirm has been deprecated, use `--yes` instead
✓ Deleted repository jwt625/test-dual-1756767639
```


2025-09-01T18:16:55-07:00
Fucking piece of shit:
```
**Bingo!** The old filtering was **skipping `README.md`** (line 1669), so there was no conflict with the auto-generated README.

Now we're trying to copy the template's README.md, which conflicts with the repository's initial README.md.
```

2025-09-01T21:15:48-07:00
Could be token scope problem...

2025-09-01T22:17:28-07:00
Fucking finally, it is indeed an oauth scope problem...
![[Pasted image 20250901221738.png]]

2025-09-01T22:30:19-07:00
A lot of conversations...
![[Pasted image 20250901223024.png]]



# Test deploy and actual deploy

2025-09-06T09:53:24-07:00

Got test deploy working, pages are actually visible.
![[Pasted image 20250906095350.png]]
- https://github.com/jwt625/test-dual-1757177047
- will probably get removed later.

Now refactoring the backend `github_service.py`.


2025-09-07T02:04:01-07:00
Removing test repos:
```

Deleting test-dual-1757224576...
Flag --confirm has been deprecated, use `--yes` instead
✓ Deleted repository jwt625/test-dual-1757224576
Deleting test-dual-1757187652...
Flag --confirm has been deprecated, use `--yes` instead
✓ Deleted repository jwt625/test-dual-1757187652
Deleting test-dual-1757187433...
Flag --confirm has been deprecated, use `--yes` instead
✓ Deleted repository jwt625/test-dual-1757187433
Deleting test-dual-1757187196...
Flag --confirm has been deprecated, use `--yes` instead
✓ Deleted repository jwt625/test-dual-1757187196
Deleting test-dual-1757186523...
Flag --confirm has been deprecated, use `--yes` instead
✓ Deleted repository jwt625/test-dual-1757186523
Deleting test-dual-1757182636...
Flag --confirm has been deprecated, use `--yes` instead
✓ Deleted repository jwt625/test-dual-1757182636
Deleting test-dual-1757177047...
Flag --confirm has been deprecated, use `--yes` instead
✓ Deleted repository jwt625/test-dual-1757177047
Deleting test-dual-1757176701...
Flag --confirm has been deprecated, use `--yes` instead
✓ Deleted repository jwt625/test-dual-1757176701
Deleting test-dual-1756789786...
Flag --confirm has been deprecated, use `--yes` instead
✓ Deleted repository jwt625/test-dual-1756789786
Deleting test-dual-1756789570...
Flag --confirm has been deprecated, use `--yes` instead
✓ Deleted repository jwt625/test-dual-1756789570
Deleting test-dual-1756789391...
Flag --confirm has been deprecated, use `--yes` instead
✓ Deleted repository jwt625/test-dual-1756789391
Deleting test-dual-1756789297...
Flag --confirm has been deprecated, use `--yes` instead
✓ Deleted repository jwt625/test-dual-1756789297
Deleting paper-18-jun-2025httpsarxivorgabs250615633v1-17572-1757234846...
Flag --confirm has been deprecated, use `--yes` instead
✓ Deleted repository jwt625/paper-18-jun-2025httpsarxivorgabs250615633v1-17572-1757234846
Deleting paper-18-jun-2025httpsarxivorgabs250615633v1-17572-1757234213...
Flag --confirm has been deprecated, use `--yes` instead
✓ Deleted repository jwt625/paper-18-jun-2025httpsarxivorgabs250615633v1-17572-1757234213
Deleting arxiv250615633v1-quant-ph-18-jun-2025httpsarxivorg-1757231870...
Flag --confirm has been deprecated, use `--yes` instead
✓ Deleted repository jwt625/arxiv250615633v1-quant-ph-18-jun-2025httpsarxivorg-1757231870
Deleting arxiv250615633v1-quant-ph-18-jun-2025httpsarxivorg...
Flag --confirm has been deprecated, use `--yes` instead
✓ Deleted repository jwt625/arxiv250615633v1-quant-ph-18-jun-2025httpsarxivorg
```









