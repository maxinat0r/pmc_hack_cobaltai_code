# pmc_hack_cobaltai_code

This repository contains all the information and code produced for the Prinses Maxima Center / Google hackathon.

## Idea

One of the problems that doctors face when they deal with families is how to communicate difficult technical or emotional information, specially if this information is directed to the children. In general, there are medical procedures that can generate anxiety or curiosity on the children. It is vital to communicate with the child to make sure the child is aware of what is about to happen and at the same time is relaxed and ready to receive the treatment. Also, having an effective communication helps the child to know what to expect after the treatment in terms of side effects.

Our idea is to use LLM to compose children's stories that help them grasp the procedures and side effects they might undergo during oncology treatments. The stories will be tuned to the children age, background, native language and children preferences. Together with the story, we want to include automatically generated pictures to illustrate the stories and create video. 

## Architecture

Interaction diagram
![Interaction Diagram](https://github.com/aocampor/pmc_hack_cobaltai_code/blob/75156805b8ea5cbe366f025bb686a5c90d8344b3/assets/figs/high_level_view_pmc.drawio.png)

## Description of the architecture

The main users of this product will be the doctor and the children parent's. The doctor will provide the medical information that needs to be understood by the child, the parents will provide personalised information of the child to be able to create a personalised outcome.

The information provided by the doctor and the parents will be put together into a json file that formats the information in a machine friendly format. Creating this format is standard practice for many developers therefore we did not focus on that during the hackathon. Productionising this product will require to design and implement an effective way to gather the input of parents and doctors.

Once the json file is in place, the json document will be feed to gemini together with a prompt that instructs gemini to generate a story in the child native's language. This story will consider all the information that the doctor inputed, and all the child preferences inputed by the parents. Gemini will then leverage this information to build up an interesting story that will communicate to the child what is about to happen. The outcome of gemini will be the story in the native language.

After the story is created, the program will generate a second prompt to be feed to gemini. This second prompt will ask gemini to chunk the story into small sections, and generate a prompt for every chunk that will be feed into a image generation model. Unfortunately for the day, we did not have the right imagen API's available, therefore we did not automate this part, but we did build the examples with external API's. 

Once you run the prompts through the image generator model, we get the images and the text that can go together. Because of time and resources reasons, we did not automate that part, but putting together the text and the images in a pdf format is something that can be automated later. 

Finally, we proposed as a final step to generate a video using the text, and the images. At this stage a final text to voice model could be used to produce the sound over the images.

## Bullet point summary 

Final product:
1. A doctor fills in patient information in a user interface (google forms or a bit more fancy front-end), fields:
  - Diagnosis (what do we know?)
  - Treatment (what are we going to do?)
  - Patient name
  - Patient age
  - Patient interests
  - Current symptoms
  - Possible side-effects treatment
2. The answers get parsed into json and stored in a google cloud storage bucket
3. Our base prompt is stored in a google cloud storage bucket (prompt)
4. Vertex AI triggers and adds the data into the spots for formatting to finalize the prompt
5. The Story is generated 
6. A new format ensures that the story is divided into a few individual prompts setting the stage for image generation
7. We sequentially generate images, with the prompts and ensure that our image style is translated from the initial till the last image
8. We put the image and text on a slides for the parents to read it as a children's book
OPTIONAL
9. Read out the text via text to speech and add some background noise. 