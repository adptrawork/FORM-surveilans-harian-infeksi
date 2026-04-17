# Auto Deployment Configuration

## Setup GitHub Actions Auto Deploy

Repository ini telah dikonfigurasi dengan GitHub Actions untuk otomatisasi deployment. Berikut adalah konfigurasi yang telah dibuat:

### 1. Build and Deploy Workflow (`.github/workflows/build-deploy.yml`)
- **Trigger**: Push ke branch `main`
- **Fungsi**:
  - Build aplikasi
  - Run tests
  - Create deployment status
  - Notifikasi melalui comment di PR

### 2. Production Deployment (`.github/workflows/deploy-prod.yml`)
- **Trigger**: Push ke branch `main` atau manual trigger
- **Fungsi**:
  - Build package
  - Deploy ke production server via SSH
  - Create backup otomatis
  - Release tag di GitHub
  - Notifikasi deployment

### 3. Linting dan Validation (`.github/workflows/deploy.yml`)
- **Trigger**: Push ke branch `main` atau PR
- **Fungsi**:
  - HTML validation
  - Code linting
  - Test runs

## Setup Production Server

### Langkah 1: Generate SSH Key
```bash
ssh-keygen -t ed25519 -C "production-server-key" -f ~/.ssh/production-server
```

### Langkah 2: Add Public Key to Server
```bash
# Copy public key
cat ~/.ssh/production-server.pub

# Pada server Linux:
sudo mkdir -p /home/git/.ssh
sudo chmod 700 /home/git/.ssh
sudo echo "your-public-key-here" >> /home/git/.ssh/authorized_keys
sudo chmod 600 /home/git/.ssh/authorized_keys
sudo chown -R git:git /home/git/.ssh
```

### Langkah 3: Setup Repository di Server
```bash
sudo mkdir -p /var/www/form-surveilans
sudo git init /var/www/form-surveilans
sudo git remote add origin https://github.com/adptrawork/FORM-surveilans-harian-infeksi.git
sudo chown -R git:git /var/www/form-surveilans
```

### Langkah 4: Configure GitHub Secrets
Di GitHub repository:
1. Settings → Secrets and variables → Actions → New repository secret

Tambahkan secrets berikut:
- `PRODUCTION_SSH_PRIVATE_KEY`: Private key yang di-generate
- `PRODUCTION_SSH_HOST`: IP address atau domain server
- `PRODUCTION_SSH_USER`: Username SSH (biasanya `git`)
- `PRODUCTION_REMOTE_PATH`: Path deployment di server

## Test Deployment Manual

Untuk test deployment manual:
1. Buka GitHub repository
2. Pilih tab Actions
3. Pilih workflow "Deploy to Production Server"
4. Klik "Run workflow"

## Monitoring Deployment

### Check Deployment Status
```bash
# Di server:
cd /var/www/form-surveilans
git log --oneline -5

# Cek backup:
ls -la /var/www/form-surveilans/backup/
```

### GitHub Notifications
- Setelah deployment, GitHub akan membuat comment di PR
- Release tag akan terbuat di GitHub Releases
- Status deployment dapat dilihat di tab Actions

## Customization

### Change Remote Path
Edit file `deploy-prod.yml` dan ubah bagian:
```yaml
REMOTE_PATH: "${{ secrets.PRODUCTION_REMOTE_PATH }}"
```

### Add Custom Commands
Tambahkan step di `deploy-prod.yml` sebelum bagian cleanup:
```yaml
- name: Run custom deployment script
  run: |
    ssh -o StrictHostKeyChecking=no ${SSH_USER}@${SSH_HOST} "
      cd ${REMOTE_PATH}
      # Custom commands here
    "
```

## Troubleshooting

### Common Issues
1. **SSH Permission Denied**: Cek authorized_keys dan permissions
2. **Deployment Failed**: Cek logs di tab Actions di GitHub
3. **File Missing**: Cek path remote di secrets

### Debug Commands
```bash
# Test SSH connection
ssh -o StrictHostKeyChecking=no ${SSH_USER}@${SSH_HOST}

# Check remote directory
ssh -o StrictHostKeyChecking=no ${SSH_USER}@${SSH_HOST} "ls -la ${REMOTE_PATH}"
```

## Security Notes
- Jangan commit private key ke repository
- Gunakan SSH key yang berbeda untuk production
- Limit access ke server hanya untuk deployment
- Update SSH key secara berkala