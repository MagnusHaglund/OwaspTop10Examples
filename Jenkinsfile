nes (27 sloc)  948 Bytes
   
node {
  stage('SCM') {
    //deleteDir()
    checkout scm
  }
  stage('Restore Packages') {
    bat "dotnet restore Blog/Blog.csproj"
  }
  stage('SonarQube Analysis') {
    def msbuildHome = tool 'Default MSBuild'
    def scannerHome = tool 'SonarScanner for MSBuild'
    withSonarQubeEnv() {
      bat "\"${scannerHome}\\SonarScanner.MSBuild.exe\" begin /k:\"Demo2\""
      bat "\"${msbuildHome}\\MSBuild.exe\" /t:restore"
      bat "\"${msbuildHome}\\MSBuild.exe\" /t:Rebuild"
      bat "\"${scannerHome}\\SonarScanner.MSBuild.exe\" end"
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
