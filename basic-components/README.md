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
### parallel and sequential builds
**note:** use **needs** key word, here docker-job depends on maven-job
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
    needs:   maven-job
    steps:
      - uses: actions/checkout@v3
      - name: Build and push
        uses: docker/build-push-action@v2
        with:
          file: ./Dockerfile # even tried with just "Dockerfile" without double quotation
          tags: myimage:latest
  ```
## summery

In this section, you learned about the core components that make up GitHub Actions. It all starts with Workflows, which are attached to GitHub repositories and define which Jobs should be executed based on specific events or triggers. A Workflow can have one or more Jobs, and each Job specifies the environment in which tasks will run, including the server and the sequence of Steps to execute. These Steps perform the actual work, which can involve running command-line commands or using predefined Actions. Jobs can run in parallel by default or sequentially if configured.

Events and triggers play a crucial role in Workflows. GitHub provides a wide variety of events, such as repository-related actions like pushing or committing changes, as well as manually starting a Workflow. A Workflow must be associated with at least one event but can support multiple events. You define Workflows in YAML files located in the .github/workflows/ directory. These files can be created directly on GitHub or locally, though they must follow the GitHub Actions syntax, utilizing specific keywords and Actions.

Jobs and their Steps execute on "runners," which are servers that handle the Jobs. GitHub provides predefined runners hosted in their data centers, supporting Linux, Windows, and macOS, with various hardware profiles based on the operating system. If the predefined runners do not meet your needs, you can bring your own runners, though that wasn't covered in this section.

Workflows execute whenever their associated event is triggered, and you can monitor their execution and logs directly on GitHub. For Steps in your Jobs, you can use the run key to execute custom shell commands, as demonstrated throughout this section. For more advanced or repetitive tasks, you can use predefined Actions, either from GitHub, the community, or your custom-built Actions. While this section covered the basics, you'll explore building custom Actions later in the course. With these fundamentals, you're ready to dive deeper into GitHub Actions.
