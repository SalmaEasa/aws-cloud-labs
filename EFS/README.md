# Amazon EFS Lab - Notes

## Steps I did
- Created security group → allowed NFS (TCP 2049).
- Built an **EFS file system** and attached it to Lab VPC.
- Used **Session Manager** to connect to EC2 instance.
- Mounted EFS → `sudo mkdir efs && sudo mount ... /efs`.
- Verified with `df -hT` → EFS mounted successfully.

## Performance Test
- Ran `fio` write test:
  - Result: **~121 MiB/s** 
  - Command used:  
    `sudo fio --name=fio-efs --filesize=10G --filename=./efs/fio-efs-test.img --bs=1M --nrfiles=1 --direct=1 --sync=0 --rw=write --iodepth=200 --ioengine=libaio`

## CloudWatch Monitoring
- Checked metrics in CloudWatch:
  - `PermittedThroughput` → [5.37 GiB/s]  
  - `DataWriteIOBytes` → [6.79 GiB]  

## Notes
- Throughput scales with storage size.
- Baseline: 50 MiB/s per TiB.
- Burst: up to 100 MiB/s (per TiB for >1 TiB FS).
