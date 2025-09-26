# Kubernetes Architecture Explained

This document provides a clear and concise overview of the fundamental architecture of Kubernetes (K8s), breaking down its core components and explaining how they interact to manage containerized applications.

## Table of Contents
1.  [Overview](#overview)
2.  [The Kubernetes Cluster](#the-kubernetes-cluster)
3.  [The Control Plane (Master Node)](#the-control-plane-master-node)
    -   [Kube API Server](#kube-api-server)
    -   [etcd](#etcd)
    -   [Kube Scheduler](#kube-scheduler)
    -   [Kube Controller Manager](#kube-controller-manager)
4.  [The Data Plane (Worker Nodes)](#the-data-plane-worker-nodes)
    -   [Kubelet](#kubelet)
    -   [Kube Proxy](#kube-proxy)
    -   [Container Runtime](#container-runtime)
5.  [How It All Works Together: An Example Workflow](#how-it-all-works-together-an-example-workflow)

---

## Overview

Kubernetes orchestrates containers by managing a cluster of machines. This architecture is designed for automation, scalability, and high availability. It can be divided into two main parts: the **Control Plane**, which manages the cluster, and the **Data Plane**, which consists of **Worker Nodes** that run the actual applications.

## The Kubernetes Cluster

A Kubernetes cluster is a set of nodes that run containerized applications. It is composed of at least one master node and multiple worker nodes.
