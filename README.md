# Kong Deck Configuration

This repository contains Kong Gateway configuration files for managing API gateway routes, services, and deployments using Kong Deck.


## Services

### Fleet Management Service
- **Name**: `fleet-management-service`
- **Host**: `fleet-management-api.aws.example.com`
- **Protocol**: HTTPS
- **Port**: 443

### Grafana Service
- **Name**: `grafana-service` 
- **Host**: `grafana-internal-alb.amazonaws.com`
- **Protocol**: HTTPS
- **Port**: 443

## Configuration Management

The Kong configuration is managed through individual YAML files that are pre-merged into `kong/kong.yaml`:

- **Services**: Defined in `kong/services/` directory
- **Routes**: Defined in `kong/routes/` directory  
- **Plugins**: Directory exists but no plugins currently configured
- **Merged Config**: Pre-generated `kong.yaml` contains the complete configuration


## GitLab CI/CD Pipeline

The project includes a comprehensive GitLab CI/CD pipeline with the following stages:

### Pipeline Stages

1. **Validate**: Validates Kong configuration using deck validate
2. **Verify**: Checks differences between current and new configuration using deck diff
3. **Backup**: Creates backup of current Kong configuration and uploads to S3
4. **Deploy**: Deploys the configuration to Kong Gateway

### Pipeline Features

- **Manual Execution**: All stages are set to manual execution for safety
- **Artifact Management**: Backup artifacts are stored for 7 days
- **S3 Backup Integration**: Automatic backup to S3 with timestamped files
- **Pre-generated Configuration**: Uses committed `kong.yaml` file instead of runtime merging
- **Comprehensive Metadata**: Backup includes GitLab commit info and environment details

### Environment Variables Required

- `KONG_ADMIN_URL`: Kong Admin API URL (default: https://<your-kong-admin-url>.com:8001)
- `DECK_VERSION`: Version of Kong Deck to use (default: 1.8.2)
- `S3_BACKUP_BUCKET`: S3 bucket for backups (default: backups-eu-central-1)
- `AWS_DEFAULT_REGION`: AWS region (default: eu-central-1)
- `AWS_ACCESS_KEY_ID`: AWS credentials for S3 access
- `AWS_SECRET_ACCESS_KEY`: AWS credentials for S3 access

## Validation and Testing

Before deployment, the pipeline automatically validates the configuration:

```bash
deck validate -s kong/kong.yaml
deck diff -s kong/kong.yaml
```

You can also run these commands locally if you have Kong Deck installed.

## Backup Strategy

The pipeline includes a comprehensive backup strategy:

- **Automated Backups**: Created before each deployment
- **S3 Storage**: Backups uploaded to configured S3 bucket
- **Timestamped Files**: Each backup includes timestamp and GitLab metadata
- **Artifact Retention**: Local artifacts kept for 7 days

## Deployment

This configuration uses Kong Deck for deployment management:

## Notes

- No authentication plugins are currently configured
- All routes use basic HTTP/HTTPS protocols 
- Configuration is managed through individual YAML files and pre-merged
- Fleet management service provides comprehensive API coverage
- Manual pipeline execution ensures controlled deployments

## Local Development

To work with this configuration locally:

1. Install Kong Deck: Follow the [official installation guide](https://docs.konghq.com/deck/installation/)
2. Validate configuration: `deck validate -s kong/kong.yaml`
3. Check differences: `deck diff -s kong/kong.yaml`
4. Deploy changes: `deck sync -s kong/kong.yaml`
