## building blocks

in github-actions we have 3 building blocks
 - workflows(jenkinsfile)
 - jobs(stages)
 - steps

 - we can add workflows to GitHub repositories and you can add as many workflows as you want to a given repository.
 - those workflows we have jobs.Now those jobs then contain one or more steps that will be executed in the order


   <img width="639" alt="{230644E8-189A-4CD5-AB10-78BD7FD4A66C}" src="https://github.com/user-attachments/assets/388a24c6-5a42-4e89-80be-794da4d41d5f" />
   

   <img width="600" alt="{95B1009B-5256-4B8C-A9AA-07999C9167FC}" src="https://github.com/user-attachments/assets/d998810a-a62f-4b3f-8920-1cc41288824d" />
### multi job pipeline
**NOte:** if we add multiple jobs we should give checkout code in every job previous job data is not applied to next job runner also we have to mention
```
name: build Workflow
on: push

jobs:
  maven-job:
    runs-on: ubuntu-latest
    steps:
      - name: checkout code
        uses: actions/checkout@v3
      - name: list of files
        run: ls
  docker-job:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Build and push
        uses: docker/build-push-action@v2
        with:
          file: ./Dockerfile # even tried with just "Dockerfile" without double quotation
          tags: myimage:latest
  ```
            
