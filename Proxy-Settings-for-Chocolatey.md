If you want chocolatey to go through a proxy, set the environment variables `http_proxy` and `https_proxy` to point to the proxy address. The format would be `address:port`. 

# Setting an environment variable in windows:

![Open system properties](https://f.cloud.github.com/assets/396205/243504/d73aa158-8a49-11e2-96b8-6c04c1a5f06c.png)

![Set environment variable](https://f.cloud.github.com/assets/396205/243529/b9636100-8a4a-11e2-9ec6-1ea6f99c9e35.png)

# What to do if My proxy is socks?

One way to go around this is to wrap the socks proxy with an http proxy. `polipo` is one such software. 
