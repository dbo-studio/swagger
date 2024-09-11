# Swagger

![Release](https://img.shields.io/github/release/gofiber/swagger.svg)
[![Discord](https://img.shields.io/badge/discord-join%20channel-7289DA)](https://gofiber.io/discord)
![Tests](https://github.com/dbo-studio/swagger/actions/workflows/test.yml/badge.svg)
![Security](https://github.com/dbo-studio/swagger/workflows/Security/badge.svg)
![Linter](https://github.com/dbo-studio/swagger/workflows/Linter/badge.svg)

fiber middleware to automatically generate RESTful API documentation with Swagger

## Usage

1. Add comments to your API source code, [See Declarative Comments Format](https://github.com/swaggo/swag#declarative-comments-format).
2. Download [Swag](https://github.com/swaggo/swag) for Go by using:
```sh
go get -u github.com/swaggo/swag/cmd/swag
# 1.16 or newer
go install github.com/swaggo/swag/cmd/swag@latest
```
3. Run the [Swag](https://github.com/swaggo/swag) in your Go project root folder which contains `main.go` file, [Swag](https://github.com/swaggo/swag) will parse comments and generate required files(`docs` folder and `docs/doc.go`).
```sh
swag init
```
4. Download [swagger](https://github.com/dbo-studio/swagger) by using:
```sh
go get -u github.com/dbo-studio/swagger
```
And import following in your code:

```go
import "github.com/dbo-studio/swagger" // swagger handler
```

### Canonical example:

```go
package main

import (
	"github.com/dbo-studio/swagger"
	"github.com/gofiber/fiber/v2"

	// docs are generated by Swag CLI, you have to import them.
	// replace with your own docs folder, usually "github.com/username/reponame/docs"
	_ "github.com/dbo-studio/swagger/example/docs"
)

// @title Fiber Example API
// @version 1.0
// @description This is a sample swagger for Fiber
// @termsOfService http://swagger.io/terms/
// @contact.name API Support
// @contact.email fiber@swagger.io
// @license.name Apache 2.0
// @license.url http://www.apache.org/licenses/LICENSE-2.0.html
// @host localhost:8080
// @BasePath /
func main() {
	app := fiber.New()

	app.Get("/swagger/*", swagger.HandlerDefault) // default

	app.Get("/swagger/*", swagger.New(swagger.Config{ // custom
		URL: "http://example.com/doc.json",
		DeepLinking: false,
		// Expand ("list") or Collapse ("none") tag groups by default
		DocExpansion: "none",
		// Prefill OAuth ClientId on Authorize popup
		OAuth: &swagger.OAuthConfig{
			AppName:  "OAuth Provider",
			ClientId: "21bb4edc-05a7-4afc-86f1-2e151e4ba6e2",
		},
		// Ability to change OAuth2 redirect uri location
		OAuth2RedirectUrl: "http://localhost:8080/swagger/oauth2-redirect.html",
	}))

	app.Listen(":8080")
}
```
5. Run it, and browser to http://localhost:8080/swagger, you can see Swagger 2.0 Api documents.
