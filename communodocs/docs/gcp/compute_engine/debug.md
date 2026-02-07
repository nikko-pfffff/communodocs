# Debug Startup Script

Directement en console
```bash
sudo journalctl -u google-startup-scripts.service -f
```

Pour relancer
```bash
sudo google_metadata_script_runner startup
```

Pour avoir les logs dans GCP il faut que le service account de l'instance ait les permissions `"roles/logging.logWriter"`
