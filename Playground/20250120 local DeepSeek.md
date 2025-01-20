
# Installation (DeepSeek-R1-Distill-Qwen-7B)
2025-01-20T11:33:22-08:00
DeepSeek just released their R1:
- https://x.com/deepseek_ai/status/1881318130334814301
Trying to get the 7B version running:
- https://x.com/awnihannun/status/1881386796266946743
	- his command:
```
mlx_lm.generate --model mlx-community/DeepSeek-R1-Distill-Qwen-7B-4bit --max-tokens 256 --prompt "Two eight-sided dice each have faces numbered 1 through 8. When the dice are rolled, each face has an equal probability of appearing on the top. What is the probability that the product of the two top numbers is greater than their sum? Express your answer as a common fraction." -m 4096
```
- In the comment people are recommending https://lmstudio.ai/ for mere mortals, maybe I should try it first.
- deepseek-ai/DeepSeek-R1-Distill-Qwen-7B
- https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Qwen-7B/tree/main


Cloning the repo
- error, git-lfs missing
- installed with homebrew
- cloned
![[Pasted image 20250120113634.png]]

Now need to figure out mlx.
- I have used exo which uses mlx but I have not directly used it before


2025-01-20T11:44:00-08:00
Trying out https://lmstudio.ai/ 
This is kind of too foolproof:
![[Pasted image 20250120114413.png]]

2025-01-20T12:05:30-08:00
Nah pretty dumb. Tried asking it to write a 2D FDTD. Going to stay with web for now.



