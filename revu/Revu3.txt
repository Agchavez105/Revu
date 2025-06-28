workspace "Revu" "Sistema de reseña de lugares" {

    model {
        # --------------------------------------------------------
        # Person
        # --------------------------------------------------------
        usuario = person "Usuario" "Persona que realiza la reseña de lugares" "Usuario"
        administrador = person "Administrador" "Persona que gestiona el software" "Administrador"

        # --------------------------------------------------------
        # softwareSystem 
        # --------------------------------------------------------
        revu = softwareSystem "Revu" "Software para realizar , mostrar y analisar reseñas sobre lugares " "Revu" {
            webApp = container "Web Application" "Single-page Application de reseñas" "Angular v18.1 / Typescript 5.6.3" "WebApp"
            mobileApp = container "Mobile Application" "App de reseñas" "Iphone / iOS / Swift 6.0" "MobileApp"

            apiGateway = container "Api Gateway" "Controlador de tráfico de solicitudes de API entrantes de back-end" "Amazon API Gateway" "ApiGateway"
            
            # --------------------------------------------------------
            # Gestion de usuario Bounded Context
            # --------------------------------------------------------
            gestionUsuarioBC = container "Gestion de usuarios Bounded  Context" "Permite al usuario registrarse y visualizar opciones" "Express.js / ECMAScript v2024" "GestionUsuarioBC,BoundedContext"{
                gestionUsuarioController = component "GestionUsuario Controller" "RESTful API Controller class de gestionUsuario" "Class / Express.js / ECMAScript v2024" "GestionUsuarioController,Controller"
    

               group "GestionUsuario" {
                    gestionUsuarioQueryService = component  "GestionUsuarios  QueryService" "Business Logic del query de GestionUsuarios" "Class / Express.js / ECMAScript v2024" "GestionUsuarioQueryService,Service"
                    gestionUsuarioCommandService = component "GestionUsuarios  CommandService" "Business Logic del Command de GestionUsuarios" "Class / Express.js / ECMAScript v2024" "GestionUsuarioCommandService,Service"
                    gestionUsuarioRepository = component "GestionUsuario Repository" "Repository Interface de GestionUsuario" "Interface / Express.js / ECMAScript v2024" "GestionUsuarioRepository,Repository"
                    gestionUsuarioeQuery = component "GestionUsuario Query" "Query for read GestionUsuario" "Record" "GestionUsuarioeQuery, Query"
                    gestionUsuarioCommand = component "GestionUsuario Command" "Command for create, update and delete GestionUsuario" "Record" "GestionUsuarioCommand, Command"

                   
               }
                group "outboundservices" {
                #
                #    gmailExternalService = component "Gmail Api ExternalService" "External Business Logic de autentificacion" "Class / Express.js / ECMAScript v2024" "GmailExternalService,ExternalService"
                #    microsoftExternalService = component "Microsoft Api ExternalService" "External Business Logic de carrito de autentificacion" "Class / Express.js / ECMAScript v2024" "MicrosoftExternalService,ExternalService"
                #    appleMailExternalService = component "Apple Mai Api ExternalService" "External Business Logic de carrito de autentificacion" "Class / Express.js / ECMAScript v2024" "AppleMailExternalService,ExternalService"
                # 
                
                    buscarExternalService = component "Buscar reseña ExternalService" "External Business Logic de buscar reseña" "Class / Express.js / ECMAScript v2024" "BuscarExternalService,ExternalService"
                    publicarExternalService = component "Publicar reseña  ExternalService" "External Business Logic de publicar reseña" "Class / Express.js / ECMAScript v2024" "PublicarExternalService,ExternalService"
                    solicitarReporteExternalService = component "Solicitar reporte ExternalService" "External Business Logic de solicitar reporte" "Class / Express.js / ECMAScript v2024" "SolicitarReporteExternalService,ExternalService"

                    
                }
               
            
             } 

            # --------------------------------------------------------
            # Busqueda de resañas Bounded Context
            # --------------------------------------------------------
            busquedaResenasBC = container "Busqueda de reseñas Bounded  Context" "Permite la busqueda de reseñas a segun las preferencias del usuario" "Express.js / ECMAScript v2024" "BusquedaReseñasBC,BoundedContext" 

            # --------------------------------------------------------
            # Lectura de reseñas Bounded Context
            # --------------------------------------------------------
            lecturaResenasBC = container "Lectura de Reseñas Bounded  Context" "Permite al usuario visualizar reseñas de otrso usuarios así como su Informacion" "Express.js / ECMAScript v2024" "LecturaReseñasBC,BoundedContext" 

            # --------------------------------------------------------
            # Publicacion de reseñas Bounded Context
            # --------------------------------------------------------
            publicacionDeResenasBC = container "Publicacion de reseñas Bounded  Context" "Permite al usurio publicar una reseña segun sus preferencias " "Express.js / ECMAScript v2024" "PublicacionDeReseñasBC,BoundedContext" 

            # --------------------------------------------------------
            # Analisis y  reportes de reseñas Bounded Context
            # --------------------------------------------------------
            analisisReportesBC = container "Analisis y Reporte Bounded  Context" "Permite realizar reportes y analis apartir de reseñas publicadas" "Express.js / ECMAScript v2024" "AnalisisReportesBC,BoundedContext" 



            # --------------------------------------------------------
            # databases
            # --------------------------------------------------------
            group "Databases" {
                gestionUsuarioDB = container "Gestion de usuarios Database" "Guarda información personal del usuario " "MongoDB Database v8.0" "GestionUsuarioDB,Database"
                busquedaResenaDB = container "Busqueda de reseñas Database" "Guarda información de las preferencias de busqueda del usuario" "MongoDB Database v8.0" "BusquedaResenaDB,Database"
                lescturaResenaDB = container "Lectura de reseñas Database" "Guarda información de las reseñas visitadas por el usuario" "Redis Database v7.4" "LescturaResenaDB,Database"
                publicicacionResenaDB = container "Publicacion de reseñas Database" "Guarda información de las reseñas publicas por el usuario" "MongoDB Database v8.0" "PublicicacionResenaDB,Database"
                analisReportesDB = container "Analis y reporte  Database" "Guarda información de los reportes y analisis" "MongoDB Database v8.0" "AnalisReportesDB,Database"

            }
        }

        # --------------------------------------------------------
        # External System
        # --------------------------------------------------------
        gmailApi = softwareSystem "Gmail Api" "Permite enviar correos a cuentas de Gmail" "Gmail Api,ExternalSystem"
        microsoftApi = softwareSystem "Microsoft Mail Api" "Permite enviar correos a cuentas de Outlook" "Microsoft Api,ExternalSystem"
        appleApi = softwareSystem "Apple Mail Api" "Permite enviar correos a cuentas de ICloud" "Apple Api,ExternalSystem"
        googleMapsApi = softwareSystem "Google Maps Api" "Ubicación Geoespacial de la dirección en el mapa" "Google Maps Api,ExternalSystem"
        googleTranslateAPI = softwareSystem "Google Translate API" "Api para traducir texto" "Google Translate API,ExternalSystem"


        # --------------------------------------------------------
        # RelationShip
        # --------------------------------------------------------

        # --------------------------------------------------------
        # Relationship systemContext
        # --------------------------------------------------------
        usuario -> revu "Reseña lugares"
        administrador -> revu "Gestiona Software"
        revu -> gmailApi "Envìa autenticación"
        revu -> microsoftApi "Envìa autenticación"
        revu -> appleApi "Envìa autenticación"
        revu -> googleMapsApi "Analisa lugares cercanos"
        revu -> googleTranslateAPI "Traduce reseñas "


        # --------------------------------------------------------
        # Relationship container
        # --------------------------------------------------------
        usuario -> webApp "Reseña lugares"
        usuario -> mobileApp "Reseña lugare"
        administrador -> webApp "Gestiona la Aplicacción"
        administrador -> mobileApp "Gestiona la Aplicacción"


        webApp -> apiGateway "Solicitudes HTTP / API"
        mobileApp -> apiGateway "HTTP Request / API"


        apiGateway -> gestionUsuarioBC "Endpoint request" "HTTPS / Json"
        apiGateway -> busquedaResenasBC "Endpoint request" "HTTPS / Json"
        apiGateway -> lecturaResenasBC "Endpoint request" "HTTPS / Json"
        apiGateway -> publicacionDeResenasBC "Endpoint request" "HTTPS / Json"
        apiGateway -> analisisReportesBC "Endpoint request" "HTTPS / Json"


        gestionUsuarioBC -> busquedaResenasBC "Cuenta creada"
        busquedaResenasBC -> lecturaResenasBC "Datos de busqueda"
        lecturaResenasBC -> analisisReportesBC "Enviar datos de reseñas visitadas"
        gestionUsuarioBC -> publicacionDeResenasBC "Cuenta creada"
        publicacionDeResenasBC -> analisisReportesBC "Enviar datos de reseñas publicadas"
        gestionUsuarioBC -> analisisReportesBC "Solicitar reporte "
        analisisReportesBC -> gestionUsuarioBC "Enviar reporte "


        gestionUsuarioBC -> gestionUsuarioDB "Guardar y recupera datos" "SQL Commands"
        busquedaResenasBC -> busquedaResenaDB "Guardar y recupera datos" "SQL Commands"
        lecturaResenasBC -> lescturaResenaDB "Guardar y recupera datos" "SQL Commands"
        publicacionDeResenasBC -> publicicacionResenaDB "Guardar y recupera datos" "SQL Commands"
        analisisReportesBC -> analisReportesDB "Guardar y recupera datos" "SQL Commands"


        gestionUsuarioBC -> gmailApi "Envìa autenticació"
        gestionUsuarioBC -> appleApi "Envìa autenticació"
        gestionUsuarioBC -> microsoftApi "Envìa autenticació"
        busquedaResenasBC -> googleMapsApi "Analisa lugares cercano"
        lecturaResenasBC -> googleTranslateAPI "Traduce Reseñas"
        
        
        # --------------------------------------------------------
        # Relationship ComponentDiagram-GestionUsuario
        # --------------------------------------------------------
        
        apiGateway -> gestionUsuarioController "Request Post del endpoint de gestion de usuarios" 
        gestionUsuarioController -> gestionUsuarioQueryService "Call service method"
        gestionUsuarioController -> gestionUsuarioCommandService "Call service method"
        gestionUsuarioQueryService -> gestionUsuarioRepository "Call repository method"
        gestionUsuarioCommandService -> gestionUsuarioRepository "Call repository method"
        gestionUsuarioRepository -> gestionUsuarioDB "Store, update, delete and fetch records - users" "SQL transaction"

        gestionUsuarioQueryService -> gestionUsuarioeQuery "Handle query" 
        gestionUsuarioCommandService -> gestionUsuarioCommand "Handle command" 

  
        gestionUsuarioCommandService -> buscarExternalService "Call service method "
        gestionUsuarioCommandService -> publicarExternalService "Call service method "
        gestionUsuarioCommandService -> solicitarReporteExternalService "Call service method "


    }
    # --------------------------------------------------------
    # View c4 Model
    # --------------------------------------------------------
    views {
        # --------------------------------------------------------
        # View SystemContext
        # --------------------------------------------------------
        systemContext revu "SystemContext" "Software para reseñar lugares visiatados por los usuarios" {
            include *
            autoLayout tb
        }

        # --------------------------------------------------------
        # View Container
        # --------------------------------------------------------
        container revu "SystemContainer" "Container del Software de soporte" {
            include *
            autoLayout tb 600 400
        }
        
        # --------------------------------------------------------
        # View ComponentDiagram-GestionUsuario
        # --------------------------------------------------------
        component gestionUsuarioBC "ComponentDiagram-GestionUsuario" "Diagrama de Componentes de Gestión de Usuarios"{
            include *
            autoLayout tb
        }

        # --------------------------------------------------------
        # Style
        # # --------------------------------------------------------
        styles {
            element "Person" {
                shape Person
                background #a3e4d7
                color #FFFFFF
            }
            element "Software System" {
                background #bb8fce
                color #FFFFFF
            }
            element "Container" {
                background #002A8D
                color #FFFFFF
            }
            element "ExternalSystem" {
                background #f5cba7
            }
            element "BoundedContext" {
                shape Component
                background #f8c471

                
            }
            element "MobileApp" {
                shape MobileDevicePortrait
                background #f2f6ff
                color #3f47e1
            }
            element "WebApp" {
                shape WebBrowser
                background #5499c7
                color #FFFFFF
                icon https://static.structurizr.com/themes/amazon-web-services-2020.04.30/Amazon-EC2_D2-Instance_light-bg@4x.png
            }
            element "LandingPage" {
                shape WebBrowser
                background #048000
                color #FFFFFF
            }
            element "ApiGateway" {
                shape Pipe
                background #e59866
            }
            element "Database" {
                shape Cylinder
                background #ec7063
            }
            element "Service" {
                shape Hexagon
                background #169923
                color #FFFFFF
            }
            element "Repository" {
                shape Diamond
                background #F0EB0A
            }
            element "Command" {
                shape Circle
                background #C22777
                color #FFFFFF
            }
            element "Query" {
                shape Ellipse
                background #42B8C2
                color #FFFFFF
            }
            element "Controller" {
                shape RoundedBox
                background #1D75F0
                color #FFFFFF
            }
            element "ExternalService" {
                shape Folder
                background #785ba1
                color #FFFFFF
            }
            element "ContextFacade" {
                shape Folder
                background #9710b1
                color #FFFFFF
            }
            element "Container" {
                shape Hexagon
                background #805B17
                color #FFFFFF
            }
            element "Group" {
                color #9a0000
            }
            element "Controller" {
                shape RoundedBox
                background #1D75F0
                color #FFFFFF
            }
            element "Repository" {
                shape Diamond
                background #F0EB0A
            }
        }
    }

    configuration {
        scope softwaresystem
    }
}