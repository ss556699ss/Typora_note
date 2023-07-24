# OMS-lineMonitoring

5.12 ExceptionHandler 繼續完成錯誤處理

如果遇到 f5 無法編譯，或無法掃到class，以及刪掉target(它好像會按照修改過的java進行編譯

所以原本的target錯的話，後面就會編譯錯了)

![image-20230522163800853](https://raw.githubusercontent.com/roger9491/Typora_note/main/img/image-20230522163800853.png)

``` json
{
    "openapi": "3.0.1",
    "info": {
        "title": "OpenAPI definition",
        "version": "v0"
    },
    "servers": [
        {
            "url": "http://172.31.136.115:11080",
            "description": "Generated server url"
        }
    ],
    "paths": {
        "/line/dispatchServices": {
            "get": {
                "tags": [
                    "main-controller"
                ],
                "operationId": "dispatchServices",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "application/json": {
                                "schema": {
                                    "type": "string"
                                }
                            }
                        }
                    }
                }
            },
            "post": {
                "tags": [
                    "main-controller"
                ],
                "operationId": "dispatchServices_1",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "application/json": {
                                "schema": {
                                    "type": "string"
                                }
                            }
                        }
                    }
                }
            }
        },
        "/line/dispatchBranchnet": {
            "get": {
                "tags": [
                    "main-controller"
                ],
                "operationId": "dispatchBranchnet",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "application/json": {
                                "schema": {
                                    "type": "string"
                                }
                            }
                        }
                    }
                }
            },
            "post": {
                "tags": [
                    "main-controller"
                ],
                "operationId": "dispatchBranchnet_1",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "application/json": {
                                "schema": {
                                    "type": "string"
                                }
                            }
                        }
                    }
                }
            }
        },
        "/line/servicesConf/update": {
            "post": {
                "tags": [
                    "main-controller"
                ],
                "operationId": "updateServicesConf",
                "requestBody": {
                    "content": {
                        "application/json": {
                            "schema": {
                                "$ref": "#/components/schemas/ServicesConf"
                            }
                        }
                    },
                    "required": true
                },
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "type": "string"
                                }
                            }
                        }
                    }
                }
            }
        },
        "/line/servicesConf/new": {
            "post": {
                "tags": [
                    "main-controller"
                ],
                "operationId": "addServicesConf",
                "requestBody": {
                    "content": {
                        "application/json": {
                            "schema": {
                                "$ref": "#/components/schemas/ServicesConf"
                            }
                        }
                    },
                    "required": true
                },
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "type": "string"
                                }
                            }
                        }
                    }
                }
            }
        },
        "/line/branchnet/update": {
            "post": {
                "tags": [
                    "main-controller"
                ],
                "operationId": "updateBranchnet",
                "requestBody": {
                    "content": {
                        "application/json": {
                            "schema": {
                                "$ref": "#/components/schemas/Branchnet"
                            }
                        }
                    },
                    "required": true
                },
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "type": "string"
                                }
                            }
                        }
                    }
                }
            }
        },
        "/line/branchnet/new": {
            "post": {
                "tags": [
                    "main-controller"
                ],
                "operationId": "addBranchnet",
                "requestBody": {
                    "content": {
                        "application/json": {
                            "schema": {
                                "$ref": "#/components/schemas/Branchnet"
                            }
                        }
                    },
                    "required": true
                },
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "type": "string"
                                }
                            }
                        }
                    }
                }
            }
        },

        "/line/updateOpStatus/{id}/{op_status}": {
            "get": {
                "tags": [
                    "main-controller"
                ],
                "operationId": "updateOpStatus_1",
                "parameters": [
                    {
                        "name": "id",
                        "in": "path",
                        "required": true,
                        "schema": {
                            "type": "string"
                        }
                    },
                    {
                        "name": "op_status",
                        "in": "path",
                        "required": true,
                        "schema": {
                            "type": "string"
                        }
                    }
                ],
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "type": "string"
                                }
                            }
                        }
                    }
                }
            }
        },
        "/line/reloadBranchnet": {
            "get": {
                "tags": [
                    "main-controller"
                ],
                "operationId": "reloadBranchnet",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "type": "string"
                                }
                            }
                        }
                    }
                }
            }
        },
        "/line/opHandle": {
            "get": {
                "tags": [
                    "main-controller"
                ],
                "operationId": "opHandle",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "type": "string"
                                }
                            }
                        }
                    }
                }
            }
        },
        "/line/changeWorkingStatus": {
            "get": {
                "tags": [
                    "main-controller"
                ],
                "operationId": "changeWorkingStatus",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "type": "string"
                                }
                            }
                        }
                    }
                }
            }
        },
        "/line/servicesConf": {
            "delete": {
                "tags": [
                    "main-controller"
                ],
                "operationId": "deleteServicesConf",
                "parameters": [
                    {
                        "name": "id",
                        "in": "query",
                        "required": true,
                        "schema": {
                            "type": "string"
                        }
                    }
                ],
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "type": "string"
                                }
                            }
                        }
                    }
                }
            }
        },
        "/line/branchnet": {
            "delete": {
                "tags": [
                    "main-controller"
                ],
                "operationId": "deleteBranchnet",
                "parameters": [
                    {
                        "name": "ip",
                        "in": "query",
                        "required": true,
                        "schema": {
                            "type": "string"
                        }
                    }
                ],
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "type": "string"
                                }
                            }
                        }
                    }
                }
            }
        },
        "/swagger-resources": {
            "get": {
                "tags": [
                    "api-resource-controller"
                ],
                "operationId": "swaggerResources",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/components/schemas/SwaggerResource"
                                    }
                                }
                            }
                        }
                    }
                }
            },
            "put": {
                "tags": [
                    "api-resource-controller"
                ],
                "operationId": "swaggerResources_3",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/components/schemas/SwaggerResource"
                                    }
                                }
                            }
                        }
                    }
                }
            },
            "post": {
                "tags": [
                    "api-resource-controller"
                ],
                "operationId": "swaggerResources_2",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/components/schemas/SwaggerResource"
                                    }
                                }
                            }
                        }
                    }
                }
            },
            "delete": {
                "tags": [
                    "api-resource-controller"
                ],
                "operationId": "swaggerResources_5",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/components/schemas/SwaggerResource"
                                    }
                                }
                            }
                        }
                    }
                }
            },
            "options": {
                "tags": [
                    "api-resource-controller"
                ],
                "operationId": "swaggerResources_6",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/components/schemas/SwaggerResource"
                                    }
                                }
                            }
                        }
                    }
                }
            },
            "head": {
                "tags": [
                    "api-resource-controller"
                ],
                "operationId": "swaggerResources_1",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/components/schemas/SwaggerResource"
                                    }
                                }
                            }
                        }
                    }
                }
            },
            "patch": {
                "tags": [
                    "api-resource-controller"
                ],
                "operationId": "swaggerResources_4",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/components/schemas/SwaggerResource"
                                    }
                                }
                            }
                        }
                    }
                }
            }
        },
        "/swagger-resources/configuration/ui": {
            "get": {
                "tags": [
                    "api-resource-controller"
                ],
                "operationId": "uiConfiguration",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "$ref": "#/components/schemas/UiConfiguration"
                                }
                            }
                        }
                    }
                }
            },
            "put": {
                "tags": [
                    "api-resource-controller"
                ],
                "operationId": "uiConfiguration_3",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "$ref": "#/components/schemas/UiConfiguration"
                                }
                            }
                        }
                    }
                }
            },
            "post": {
                "tags": [
                    "api-resource-controller"
                ],
                "operationId": "uiConfiguration_2",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "$ref": "#/components/schemas/UiConfiguration"
                                }
                            }
                        }
                    }
                }
            },
            "delete": {
                "tags": [
                    "api-resource-controller"
                ],
                "operationId": "uiConfiguration_5",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "$ref": "#/components/schemas/UiConfiguration"
                                }
                            }
                        }
                    }
                }
            },
            "options": {
                "tags": [
                    "api-resource-controller"
                ],
                "operationId": "uiConfiguration_6",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "$ref": "#/components/schemas/UiConfiguration"
                                }
                            }
                        }
                    }
                }
            },
            "head": {
                "tags": [
                    "api-resource-controller"
                ],
                "operationId": "uiConfiguration_1",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "$ref": "#/components/schemas/UiConfiguration"
                                }
                            }
                        }
                    }
                }
            },
            "patch": {
                "tags": [
                    "api-resource-controller"
                ],
                "operationId": "uiConfiguration_4",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "$ref": "#/components/schemas/UiConfiguration"
                                }
                            }
                        }
                    }
                }
            }
        },
        "/swagger-resources/configuration/security": {
            "get": {
                "tags": [
                    "api-resource-controller"
                ],
                "operationId": "securityConfiguration",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "$ref": "#/components/schemas/SecurityConfiguration"
                                }
                            }
                        }
                    }
                }
            },
            "put": {
                "tags": [
                    "api-resource-controller"
                ],
                "operationId": "securityConfiguration_3",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "$ref": "#/components/schemas/SecurityConfiguration"
                                }
                            }
                        }
                    }
                }
            },
            "post": {
                "tags": [
                    "api-resource-controller"
                ],
                "operationId": "securityConfiguration_2",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "$ref": "#/components/schemas/SecurityConfiguration"
                                }
                            }
                        }
                    }
                }
            },
            "delete": {
                "tags": [
                    "api-resource-controller"
                ],
                "operationId": "securityConfiguration_5",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "$ref": "#/components/schemas/SecurityConfiguration"
                                }
                            }
                        }
                    }
                }
            },
            "options": {
                "tags": [
                    "api-resource-controller"
                ],
                "operationId": "securityConfiguration_6",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "$ref": "#/components/schemas/SecurityConfiguration"
                                }
                            }
                        }
                    }
                }
            },
            "head": {
                "tags": [
                    "api-resource-controller"
                ],
                "operationId": "securityConfiguration_1",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "$ref": "#/components/schemas/SecurityConfiguration"
                                }
                            }
                        }
                    }
                }
            },
            "patch": {
                "tags": [
                    "api-resource-controller"
                ],
                "operationId": "securityConfiguration_4",
                "responses": {
                    "200": {
                        "description": "OK",
                        "content": {
                            "*/*": {
                                "schema": {
                                    "$ref": "#/components/schemas/SecurityConfiguration"
                                }
                            }
                        }
                    }
                }
            }
        }
    },
    "components": {
        "schemas": {
            "ServicesConf": {
                "type": "object",
                "properties": {
                    "id": {
                        "type": "integer",
                        "format": "int64"
                    },
                    "server_host_ip_port": {
                        "type": "string"
                    },
                    "server_id": {
                        "type": "string"
                    },
                    "server_host_name": {
                        "type": "string"
                    },
                    "server_host_ip": {
                        "type": "string"
                    },
                    "server_hardware": {
                        "type": "string"
                    },
                    "msgid_s4XX": {
                        "type": "string"
                    },
                    "msgid_s5XX": {
                        "type": "string"
                    },
                    "socket_check": {
                        "type": "string"
                    },
                    "socket_port": {
                        "type": "string"
                    },
                    "socket_msgid": {
                        "type": "string"
                    },
                    "socket_oms_note": {
                        "type": "string"
                    },
                    "http_url": {
                        "type": "string"
                    },
                    "http_check": {
                        "type": "string"
                    },
                    "http_check_count": {
                        "type": "string"
                    },
                    "http_oms_note": {
                        "type": "string"
                    }
                }
            },
            "Branchnet": {
                "type": "object",
                "properties": {
                    "ip": {
                        "type": "string"
                    },
                    "branch_id": {
                        "type": "string"
                    },
                    "branch_name": {
                        "type": "string"
                    },
                    "action": {
                        "type": "string"
                    },
                    "remark": {
                        "type": "string"
                    },
                    "describe": {
                        "type": "string"
                    },
                    "group": {
                        "maxLength": 1,
                        "minLength": 1,
                        "type": "string"
                    },
                    "hostname": {
                        "type": "string"
                    },
                    "repair_info": {
                        "type": "string"
                    },
                    "interfaces": {
                        "type": "string"
                    },
                    "tel": {
                        "type": "string"
                    },
                    "tfn_ip": {
                        "type": "string"
                    },
                    "machine": {
                        "type": "string"
                    },
                    "interfaces_info": {
                        "type": "string"
                    },
                    "sequence": {
                        "type": "string"
                    },
                    "place": {
                        "type": "string"
                    },
                    "router_ip": {
                        "type": "string"
                    },
                    "type": {
                        "type": "string"
                    },
                    "backup_tel": {
                        "type": "string"
                    },
                    "gateway": {
                        "type": "string"
                    }
                }
            },
            "SwaggerResource": {
                "type": "object",
                "properties": {
                    "name": {
                        "type": "string"
                    },
                    "url": {
                        "type": "string"
                    },
                    "swaggerVersion": {
                        "type": "string"
                    },
                    "location": {
                        "type": "string",
                        "deprecated": true
                    }
                }
            },
            "UiConfiguration": {
                "type": "object",
                "properties": {
                    "deepLinking": {
                        "type": "boolean"
                    },
                    "displayOperationId": {
                        "type": "boolean"
                    },
                    "defaultModelsExpandDepth": {
                        "type": "integer",
                        "format": "int32"
                    },
                    "defaultModelExpandDepth": {
                        "type": "integer",
                        "format": "int32"
                    },
                    "defaultModelRendering": {
                        "type": "string",
                        "enum": [
                            "example",
                            "model"
                        ]
                    },
                    "displayRequestDuration": {
                        "type": "boolean"
                    },
                    "docExpansion": {
                        "type": "string",
                        "enum": [
                            "none",
                            "list",
                            "full"
                        ]
                    },
                    "filter": {
                        "type": "object"
                    },
                    "maxDisplayedTags": {
                        "type": "integer",
                        "format": "int32"
                    },
                    "operationsSorter": {
                        "type": "string",
                        "enum": [
                            "alpha",
                            "method"
                        ]
                    },
                    "showExtensions": {
                        "type": "boolean"
                    },
                    "tagsSorter": {
                        "type": "string",
                        "enum": [
                            "alpha"
                        ]
                    },
                    "validatorUrl": {
                        "type": "string"
                    },
                    "apisSorter": {
                        "type": "string",
                        "deprecated": true
                    },
                    "jsonEditor": {
                        "type": "boolean",
                        "deprecated": true
                    },
                    "showRequestHeaders": {
                        "type": "boolean",
                        "deprecated": true
                    },
                    "supportedSubmitMethods": {
                        "type": "array",
                        "items": {
                            "type": "string"
                        }
                    }
                }
            },
            "SecurityConfiguration": {
                "type": "object",
                "properties": {
                    "apiKey": {
                        "type": "string",
                        "deprecated": true
                    },
                    "apiKeyVehicle": {
                        "type": "string",
                        "deprecated": true
                    },
                    "apiKeyName": {
                        "type": "string",
                        "deprecated": true
                    },
                    "clientId": {
                        "type": "string"
                    },
                    "clientSecret": {
                        "type": "string"
                    },
                    "realm": {
                        "type": "string"
                    },
                    "appName": {
                        "type": "string"
                    },
                    "scopeSeparator": {
                        "type": "string"
                    },
                    "additionalQueryStringParams": {
                        "type": "object",
                        "additionalProperties": {
                            "type": "object"
                        }
                    },
                    "useBasicAuthenticationWithAccessCodeGrant": {
                        "type": "boolean"
                    }
                }
            }
        }
    }
}
```





branchent	ping 

branchnet

​	線路詳細資訊

pingstatus

​	監控時發生

server 	ip + port

services_conf

status

監控不一樣



branchnet

pingstatus

servuces_conf

status



![image-20230607101555235](https://raw.githubusercontent.com/roger9491/Typora_note/main/img/image-20230607101555235.png)
