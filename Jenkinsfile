pipeline {
    agent any

    stages {
        stage('Build and Push Image') {
            steps {
                sh 'docker build -t Devops2 . && docker push Devops2'
            }
        }
        stage('Deploy to Jenkins') {
            steps {
                script {
                    def server = 'http://13.51.45.88:8080/'
                    def username = 'crypto2kama'
                    def password = 'Q1ilfcccMJ'
                    def jobName = 'kama2'

                    withCredentials([
                        usernamePassword(credentialsId: 'jenkins-credentials', username: crypto2kama, password: Q1ilfcccMJ)
                    ]) {
                        def connection = new URL(server).openConnection()
                        connection.setRequestProperty('Authorization', 'Basic ' + Base64.getEncoder().encodeToString("${username}:${password}".getBytes()))
                        connection.doOutput = true
                        connection.connect()

                        def xml = """
                        <job>
                            <name>${jobName}</name>
                            <description></description>
                            <displayName>${jobName}</displayName>
                            <buildSteps>
                                <dockerComposeStep>
                                    <containers>
                                        <container>
                                            <name>web</name>
                                            <image>project_name</image>
                                            <portBindings>
                                                <string>8000:8000</string>
                                            </portBindings>
                                            <environment>
                                                <string>DJANGO_SETTINGS_MODULE=composeexample.settings</string>
                                                <string>DATABASE_URL=postgres://postgres:password@db:5432/your_database_name</string>
                                            </environment>
                                        </container>
                                        <container>
                                            <name>db</name>
                                            <image>postgres:14-alpine</image>
                                            <environment>
                                                <string>POSTGRES_USER=postgres</string>
                                                <string>POSTGRES_PASSWORD=password</string>
                                                <string>POSTGRES_DB=your_database_name</string>
                                            </environment>
                                            <volumes>
                                                <volume>
                                                    <name>postgres_data</name>
                                                    <hostPath>/path/to/persistent/volume</hostPath>
                                                </volume>
                                            </volumes>
                                        </container>
                                    </containers>
                                </dockerComposeStep>
                            </buildSteps>
                        </job>
                        """

                        connection.outputStream.write(xml.getBytes())
                        connection.outputStream.close()
                    }
                }
            }
        }
    }
}
