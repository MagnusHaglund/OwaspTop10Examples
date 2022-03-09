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
      //bat "C:\\Tools\\sonar-scanner-4.6.2.2472\\bin\\sonar-scanner.bat begin /k:\"DepTrackSonarQubeDEMO\""
      //bat "SonarQube.Scanner.MSBuild.exe begin /k:\"DepTrackSonarQubeDEMO\""
      //bat "C:\\Tools\\sonar-scanner-msbuild-5.5.3.43281-net46\\SonarScanner.MSBuild.exe begin /k:\"DepTrackSonarQubeDEMO\""
      //bat "MSBuild.exe /t:Rebuild"
      //bat "C:\\Tools\\sonar-scanner-4.6.2.2472\\bin\\sonar-scanner.bat end"
      //bat "C:\\Tools\\sonar-scanner-msbuild-5.5.3.43281-net46\\SonarScanner.MSBuild.exe end"
      dotnet C:\\Tools\\sonar-scanner-msbuild-5.5.3.43281-netcoreapp2.0\\SonarScanner.MSBuild.dll begin /k:"DepTrackSonarQubeDEMO"
      dotnet build Blog.sln
      dotnet C:\\Tools\\sonar-scanner-msbuild-5.5.3.43281-netcoreapp2.0\\SonarScanner.MSBuild.dll end
    }
  }
  stage ('Generating Software Bill of Materials') {
    //bat "dotnet tool install --global CycloneDX"
    bat "dotnet CycloneDX Blog.sln -o ."
  }
  stage('dependencyTrackPublisher') {
    dependencyTrackPublisher artifact: 'bom.xml', projectId: '123530a3-16ef-4629-ac6d-dcac3ba6514c', synchronous: true
  }
}
