node {
  stage('SCM') {
    //deleteDir()
    checkout scm
  }
  stage('Restore Packages') {
    bat "dotnet restore Blog/Blog.csproj"
  }
  stage('SonarQube Analysis') {  
    //withSonarQubeEnv() {
      //bat "SonarScanner.MSBuild.exe begin /k:\"DepTrackSonarQubeDEMO\""
      //bat "MSBuild.exe /t:Rebuild"
      //bat "SonarScanner.MSBuild.exe end"
      bat "dotnet sonarscanner begin /k:\"DepTrackSonarQubeDEMO\""
      bat "dotnet build Blog.sln"
      bat "dotnet sonarscanner end"
    //}
    
  }
  stage ('Generating Software Bill of Materials') {
    //bat "dotnet tool install --global CycloneDX"
    bat "dotnet CycloneDX Blog.sln -o ."
  }
  stage('dependencyTrackPublisher') {
    dependencyTrackPublisher artifact: 'bom.xml', projectId: '123530a3-16ef-4629-ac6d-dcac3ba6514c', synchronous: true
  }
}
