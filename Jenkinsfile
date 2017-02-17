final IMAGE_BASE_NAME = 'camptocamp/jenkins-conf'
final DOCKER_REGISTRY_URL = 'https://registry.hub.docker.com'

// enable trigger on push
properties([
    pipelineTriggers([
      [$class: "GitHubPushTrigger"]
    ])
])


ansiColor('xterm') {
    node("docker") {
        sh 'git describe --abbrev=0 --tags > last-tag'
        def tag = readFile('last-tag').trim()

        stage "build docker image"
        def app = docker.build "${IMAGE_BASE_NAME}:${tag}"

        stage "publish docker image"
        withCredentials(
            [[$class: 'UsernamePasswordMultiBinding',
            credentialsId: 'dockerhubc2c',
            usernameVariable: 'USERNAME',
            passwordVariable: 'PASSWORD']]
        ){
            sh 'docker login -u "$USERNAME" -p "$PASSWORD"'
            docker.image("${IMAGE_BASE_NAME}:${tag}").push()
            sh 'rm last-tag'
        }
    }
}