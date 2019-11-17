---
categories: [tech, Dev]
tags: [go, Microservice]
---

```go-oauth2``` is a pkg for oauth2 features. We could build a oauth2 server by modifying the hanlder functions to verify. See the code below:

```go
package main

import (
	"net/http"

	"github.com/gin-gonic/gin"
	ginserver "github.com/go-oauth2/gin-server"
	"gopkg.in/oauth2.v3/manage"
	"gopkg.in/oauth2.v3/models"
	"gopkg.in/oauth2.v3/server"
	"gopkg.in/oauth2.v3/store"
)

func main() {
	manager := manage.NewDefaultManager()

	// token store
	manager.MustTokenStorage(store.NewFileTokenStore("data.db"))

	// client store
	clientStore := store.NewClientStore()
	clientStore.Set("000000", &models.Client{
		ID:     "000000",
		Secret: "999999",
		Domain: "http://localhost",
	})
	manager.MapClientStorage(clientStore)

	// Initialize the oauth2 service
	gs := ginserver.InitServer(manager)
	ginserver.SetAllowGetAccessRequest(true)
	ginserver.SetClientInfoHandler(server.ClientFormHandler)

	g := gin.Default()

	gs.PasswordAuthorizationHandler = func(username, password string) (string, error) {
		// This is the main part to modify
		return "user_id_example", nil
	}

	auth := g.Group("/oauth2")
	{
		auth.GET("/token", ginserver.HandleTokenRequest)
	}

	api := g.Group("/api")
	{
		api.Use(ginserver.HandleTokenVerify())
		api.GET("/test", func(c *gin.Context) {
			ti, exists := c.Get(ginserver.DefaultConfig.TokenKey)
			if exists {
				c.JSON(http.StatusOK, ti)
				return
			}
			c.String(http.StatusOK, "not found")
		})
	}

	g.Run(":9096")
}
```

It's convienient to build a new server for authorize. But the server may query the DB to verify. As we know, DB doesn't allow app to visit as secure reasons.
So we should rebuild this to Microservice version:
- call remote api to verify
- config the remote api in env
- implement the remote api in the service App

In this way, We could easily run the authorize service by config, not by modifying the code. Not with DB, The server will be very pure  and will be painless to maintain.
