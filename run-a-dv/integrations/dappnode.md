# DappNode

For setup, see quickstart guide:[​](https://docs.obol.org/next/run/integrations/Dappnode#for-setup-see-quickstart-guide)

For setup of a DV using Dappnode, see the quickstart guide [Create a DV Alone](https://docs.obol.org/next/run/start/quickstart_alone), and select the appropriate tab for "Dappnode".

### Frequently asked questions[​](https://docs.obol.org/next/run/integrations/Dappnode#frequently-asked-questions) <a href="#frequently-asked-questions" id="frequently-asked-questions"></a>

#### If an operator uses an ENR to join a cluster, then exits the validator key, do they need to clean up the validator and Charon volumes to use the same ENR for another cluster?[​](https://docs.obol.org/next/run/integrations/Dappnode#if-an-operator-uses-an-enr-to-join-a-cluster-then-exits-the-validator-key-do-they-need-to-clean-up-the-validator-and-charon-volumes-to-use-the-same-enr-for-another-cluster) <a href="#if-an-operator-uses-an-enr-to-join-a-cluster-then-exits-the-validator-key-do-they-need-to-clean-up-t" id="if-an-operator-uses-an-enr-to-join-a-cluster-then-exits-the-validator-key-do-they-need-to-clean-up-t"></a>

Yes, they need to clean up the Charon and validator volumes. However, instead of deleting everything, the operator can:

1. Download a backup (keep a copy just in case).
2. Edit the backup, keeping only the necessary files from the specific cluster (see image below)

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

3. Recompress the edited backup and upload it again after removing the Charon and validator volumes.

#### Does an operator need to use the `VALIDATOR_EXTRA_OPTS` to pass the `builderonly` or `builderalways` flag for Lodestar VC?[​](https://docs.obol.org/next/run/integrations/Dappnode#does-an-operator-need-to-use-the-validator_extra_opts-to-pass-the-builderonly-or-builderalways-flag-for-lodestar-vc) <a href="#does-an-operator-need-to-use-the-validator_extra_opts-to-pass-the-builderonly-or-builderalways-flag" id="does-an-operator-need-to-use-the-validator_extra_opts-to-pass-the-builderonly-or-builderalways-flag"></a>

No, if `ENABLE_MEV_BOOST` is set to `true`, these flags will be added automatically.\\

<figure><img src="../../.gitbook/assets/image (90).png" alt=""><figcaption></figcaption></figure>

#### How can users running two clusters (e.g., one for EtherFi solo stakers and another for Techne) on the same Dappnode machine push monitoring data from both clusters to Obol?[​](https://docs.obol.org/next/run/integrations/Dappnode#how-can-users-running-two-clusters-eg-one-for-etherfi-solo-stakers-and-another-for-techne-on-the-same-dappnode-machine-push-monitoring-data-from-both-clusters-to-obol) <a href="#how-can-users-running-two-clusters-eg-one-for-etherfi-solo-stakers-and-another-for-techne-on-the-sam" id="how-can-users-running-two-clusters-eg-one-for-etherfi-solo-stakers-and-another-for-techne-on-the-sam"></a>

In the Config tab, there is a field called "Charons to monitor by Obol (optional)". You just need to enter the cluster numbers you are using in Dappnode. For example, if you’re running three nodes on clusters 1, 2, and 3, you would enter “1,2,3”.
