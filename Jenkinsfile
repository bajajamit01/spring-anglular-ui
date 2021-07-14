#!/usr/bin/groovy

podTemplate(label: 'prismaCloud-example-builder', // See 1
  containers: [
    containerTemplate(
      name: 'jnlp',
      image: 'jenkinsci/jnlp-slave:3.10-1-alpine',
      args: '${computer.jnlpmac} ${computer.name}'
    ),
    containerTemplate(
      name: 'alpine',
      image: 'twistian/alpine:latest',
      command: 'cat',
      ttyEnabled: true
    ),
  ],
  volumes: [ // See 2
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'), // See 3
  ]
)
{
  node ('prismaCloud-example-builder') {

    stage ('Pull image') { // See 4
      container('alpine') {
        sh " curl --unix-socket /var/run/docker.sock -X POST 'http:/v1.24/images/create?fromImage=nginx:stable-alpine' "
      }
    }

    stage ('Prisma Cloud scan') { // See 6
        prismaCloudImageScan ca: '',
                    cert: '',
                    dockerAddress: 'unix:///var/run/docker.sock',
                    image: 'nginx:stable-alpine',
                    resultsFile: 'prisma-cloud-scan-results.xml',
                    project: '',
                    dockerAddress: 'unix:///var/run/docker.sock',
                    ignoreImageBuildTime: true,
                    key: '',
                    logLevel: 'info',
                    podmanPath: '',
                    project: '',
                    resultsFile: 'prisma-cloud-scan-results.json',
                    ignoreImageBuildTime:true
    }

    stage ('Prisma Cloud publish') {
        twistlockPublish resultsFilePattern: 'prisma-cloud-scan-results.json'
    }
  }
}
