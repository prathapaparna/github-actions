# Upload-Download-artifacts
```
name: Build and Deploy Workflow
on: push

jobs:
  maven-job:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Build with Maven
        run: mvn clean install

      - name: Upload Artifacts
        uses: actions/upload-artifact@v4
        with:
          name: target-files
          path: target

  docker-job:
    needs: maven-job
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code (To get Dockerfile)
        uses: actions/checkout@v3

      - name: Get Build Artifacts
        uses: actions/download-artifact@v4
        with:
          name: target-files
          path: target

      - name: Build Docker Image
        run: docker build -t my-java-app .

      - name: Run Docker Container
        run: docker run -d --name my-running-app my-java-app
```
-  maven job uploading artifacts into target folder and(we can manually download and also used in another job)
-  docker job downloading automatically those artifacts

# Job Output
- if you want to pass one job output(it can be a value, status, anything) to another job then we can use job outputs
- **ex:** we might want to pass the container's status or URL from one job to another. You can achieve this using job outputs.
```
name: Build and Deploy Workflow
on: push

jobs:
  maven-job:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Build with Maven
        run: mvn clean install

      - name: Upload Artifacts
        uses: actions/upload-artifact@v4
        with:
          name: target-files
          path: target

  docker-job:
    needs: maven-job
    runs-on: ubuntu-latest
    outputs:
      container_status: ${{ steps.check_status.outputs.status }}  # Setting job output
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Get Build Artifacts
        uses: actions/download-artifact@v4
        with:
          name: target-files
          path: target

      - name: Build Docker Image
        run: docker build -t my-java-app .

      - name: Run Docker Container
        run: docker run -d -p 8080:8080 --name my-running-app my-java-app

      - name: Check if the Container is Running
        id: check_status
        run: |
          if docker ps | grep my-running-app; then
            echo "status=running" >> "$GITHUB_ENV"
          else
            echo "status=failed" >> "$GITHUB_ENV"
          fi

  verify-container:
    needs: docker-job  # Runs after docker-job
    runs-on: ubuntu-latest
    steps:
      - name: Verify Job Output
        run: echo "The container status is ${{ needs.docker-job.outputs.container_status }}"
```
**Explanation of Job Output Usage**

- Set Job Output in docker-job

- The step **check_status** checks if the container is running.It assigns "running" or "failed" to the status variable.This is passed as an output in **outputs.container_status**.Use Job Output in verify-container

- needs: docker-job makes sure it runs only after docker-job.It prints the container status from **${{ needs.docker-job.outputs.container_status }}**.
- 
- Using **$GITHUB_OUTPUT** instead of **$GITHUB_ENV**

**$GITHUB_OUTPUT** ensures the value is stored as a job output.
**$GITHUB_ENV** is used for setting environment variables, not job outputs.

# cache dependencies
- Caching dependencies helps reduce build times by storing and **reusing previously downloaded dependencies**. Since your workflow involves build and test, we can cache dependencies for both.
- 🔹 Benefits of Caching
- 🚀 Reduces build times (especially for Maven & Docker builds).
- ✅ Avoids redundant downloads of dependencies.
- 🔥 Improves CI/CD performance in GitHub Actions.
```
- name: Cache Maven Dependencies
  uses: actions/cache@v3
  with:
    path: ~/.m2/repository
    key: maven-${{ runner.os }}-${{ hashFiles('**/pom.xml') }}
    restore-keys: |
      maven-${{ runner.os }}-
```
✅ How It Works?

- Caches .m2/repository (Maven dependencies directory).
- If pom.xml changes, dependencies will not be redownloaded.
- If cache is available, it restores dependencies automatically.

**first-run**

 <img width="577" alt="{9EE74D20-AA56-4249-9925-048690CAE9C5}" src="https://github.com/user-attachments/assets/c4e7786d-c148-410b-96f9-983530e766a9" />
 <img width="187" alt="{3952A5C3-C06C-4BE2-8E7F-585A05D82039}" src="https://github.com/user-attachments/assets/21e2880e-5ab5-4fa8-ade1-33b11389bb9a" />
 
 - during first run maven job taking 2m

  
**re-run**
  <img width="700" alt="{E41F05D6-E4FB-4E7C-A1F4-F702841FCD94}" src="https://github.com/user-attachments/assets/148c2b12-254f-4b72-a39c-4143571bb2ab" />
  <img width="188" alt="{7AA01C20-505A-47A6-9AA8-02C2E8B32D52}" src="https://github.com/user-attachments/assets/3f5852a3-28d9-4c87-8812-64d7f32fa743" />
  
  - during rerun maven job taking 13s





  

