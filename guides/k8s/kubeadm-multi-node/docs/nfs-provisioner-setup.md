# NFS Dynamic Provisioning

Use the NFS Subdir External Provisioner to enable dynamic provisioning of Persistent Volumes using an existing NFS server.

## Install NFS Subdir External Provisioner

Add the Helm repository:

```bash
helm repo add nfs-subdir-external-provisioner https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner/
helm repo update
```

Install the provisioner:

```bash
helm install nfs-subdir-external-provisioner nfs-subdir-external-provisioner/nfs-subdir-external-provisioner \\
    --set nfs.server=<nfs_server_ip> \\
    --set nfs.path=<nfs_export_path>
```

Replace `<nfs_server_ip>` and `<nfs_export_path>` with your NFS server details.

## Configure StorageClass (Optional)

You may need to configure the StorageClass to meet your requirements. Refer to the [NFS Subdir External Provisioner Documentation](https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner) for more information.
