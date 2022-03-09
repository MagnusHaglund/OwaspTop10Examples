node {
  stage('SCM') {
    //deleteDir()
    checkout scm
  }
  stage('Restore Packages') {
    bat "dotnet restore Blog/Blog.csproj"
  }
  stage('SonarQube Analysis') {  
    withSonarQubeEnv() {
      bat "dotnet-sonarscanner begin /k:\"DepTrackSonarQubeDEMO\""
      bat "MSBuild.exe /t:restore"
      bat "MSBuild.exe /t:Rebuild"
      bat "dotnet-sonarscanner end"
    }
  }
  stage ('Generating Software Bill of Materials') {
    //bat "dotnet tool install --global CycloneDX"
    bat "dotnet CycloneDX Blog.sln -o ."
  }
  stage('dependencyTrackPublisher') {
    dependencyTrackPublisher artifact: 'bom.xml', projectId: 'e23b62c2-fc91-4572-8185-0234bd7220e6', synchronous: true
  }
}
