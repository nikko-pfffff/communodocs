# Exposer un service local

Il est possible d'exposer un service local un serveur MCP, un serveur Webhook ou un container...
<br>
<br>
Soit de façon `permanente` avec un nom DNS dynamique, un proxy Caddy, et la génération du certificat Let's Encrypt.<br>
L'exemple suivra prochainement.
<br>
<br>
Soit juste pour le test via un tunnel Cloudflare, sans aucune ouverture ou confguration firewall.<br>
Il faut bien sur créer un compte Cloudflare auparavant.
<br>
On obtient une url https temporaire, le temps de l'ouverture du tunnel...<br>
```bash
➜  cloudflared tunnel --url http://localhost:8080
2026-02-07T20:58:59Z INF Thank you for trying Cloudflare Tunnel. Doing so, without a Cloudflare account, is a quick way to experiment and try it out. However, be aware that these account-less Tunnels have no uptime guarantee, are subject to the Cloudflare Online Services Terms of Use (https://www.cloudflare.com/website-terms/), and Cloudflare reserves the right to investigate your use of Tunnels for violations of such terms. If you intend to use Tunnels in production you should use a pre-created named tunnel by following: https://developers.cloudflare.com/cloudflare-one/connections/connect-apps
2026-02-07T20:58:59Z INF Requesting new quick Tunnel on trycloudflare.com...
2026-02-07T20:59:04Z INF +--------------------------------------------------------------------------------------------+
2026-02-07T20:59:04Z INF |  Your quick Tunnel has been created! Visit it at (it may take some time to be reachable):  |
2026-02-07T20:59:04Z INF |  https://passive-dried-marc-glad.trycloudflare.com                                         |
2026-02-07T20:59:04Z INF +--------------------------------------------------------------------------------------------+
```