
# Background

2024-06-28T12:44:42-07:00
Trying to find if there is any dataset that could detect common lab tools and objects
- RF, microwave
- vacuum, cryogenics
- fiber optics
- equipments
- common tools: screw drivers, wrenches


Existing datasets:
- https://scale.com/blog/best-10-public-datasets-object-detection
- https://public.roboflow.com/object-detection

Existing works are almost 10 years old:
- https://paperswithcode.com/paper/visual-genome-connecting-language-and-vision

Some more recent work:
- https://github.com/leolyliu/TACO-Instructions


Is there no one working on the long tail problem??

SAM should help identifying the boundaries.

# Advices
2024-07-04T20:05:03-07:00 Summarizing advice I got from DVD here
> Yes, using SAM to segment the objects and then label them sounds like the right approach. The SAM results look great! It would be hard to collect a high-quality, diverse dataset, though. Many dataset builders nowadays gather images online, and I'm not sure if it would be hard to collect enough lab images.

> I guess maybe you can try few-shot object detection models like DeVit ([https://github.com/mlzxy/devit](https://t.co/bec1PgRB2R)) and see how it performs


> Yes, I think 100 images would be enough to recognize large objects. Using rendered objects is an interesting idea. I think OpenAI is actively using this technique for their generative models, although I think it would be hard to batch render images that are both diverse and realistic.
> I format the dataset into COCO format, then finetune a pre-trained model on the Stanford cluster.
> 
>I previously used EVA-2 ([https://github.com/baaivision/EVA](https://t.co/7NcIUYnMy3)). They provided fine-tuning scripts, so basically, I just added the dataset to the existing fine-tuning script and then ran it.

Paper:
- https://arxiv.org/abs/2402.04252




## COCO format
2024-07-04T20:06:30-07:00

What is COCO format?


