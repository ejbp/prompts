Access to fetch at 'https://api.xema.dev/projects' from origin 'https://7b24f6cd-2552-4f9f-bb89-3d98aa18c1d2.lovableproject.com' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource.Understand this error

you should allow cors from any *.lovableproject.com, xema.dev, *.xema.dev, localhost:*

You should add to .env, .env.example and main.values.yaml too sot that i can easily update those

I want to be able to add like this:

name: CORS_ALLOWED_ORIGINS
value: ".lovableproject.com,xema.dev,*.xema.dev"