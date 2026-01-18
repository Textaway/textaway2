"use client";

import React, { useState, useEffect } from "react";
import { ActivityLog } from "@/entities/ActivityLog";
import useAppLevelAuth from "@/hooks/useAppLevelAuth";
import { useRouter } from "next/navigation";
import { createPageUrl } from "@/utils";
import { ArrowLeft, Shield } from "lucide-react";
import { Button } from "@/components/ui/button";
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";
import { Table, TableBody, TableCell, TableHead, TableHeader, TableRow } from "@/components/ui/table";
import { format } from "date-fns";

export default function SecurityPage() {
  const { isLoggedIn } = useAppLevelAuth();
  const router = useRouter();
  const [activityLogs, setActivityLogs] = useState<any[]>([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    if (isLoggedIn) {
      const fetchLogs = async () => {
        try {
          setLoading(true);
          const logs = await ActivityLog.list("createdAt:desc", 50);
          setActivityLogs(logs || []);
        } catch (error) {
          console.error("Failed to fetch activity logs:", error);
        } finally {
          setLoading(false);
        }
      };
      fetchLogs();
    }
  }, [isLoggedIn]);

  if (!isLoggedIn) {
    return null;
  }

  return (
    <div className="container mx-auto px-4 py-8 pb-24 md:pb-8">
      <div className="max-w-4xl mx-auto">
        <div className="mb-8">
          <Button
            variant="ghost"
            onClick={() => router.push(createPageUrl('ProfileSettings'))}
            className="mb-4 text-neutral-400 hover:text-neutral-50"
          >
            <ArrowLeft className="h-4 w-4 mr-2" />
            Back to Settings
          </Button>
          <h1 className="text-3xl font-bold mb-2">Security & Activity</h1>
          <p className="text-neutral-400">Review recent activity on your account</p>
        </div>

        <Card className="bg-neutral-900 border-neutral-800">
          <CardHeader>
            <CardTitle className="flex items-center gap-2">
              <Shield className="h-5 w-5" />
              Recent Activity
            </CardTitle>
          </CardHeader>
          <CardContent>
            <div className="overflow-x-auto">
              <Table>
                <TableHeader>
                  <TableRow className="border-neutral-800 hover:bg-neutral-900">
                    <TableHead>Action</TableHead>
                    <TableHead>Details</TableHead>
                    <TableHead>Device</TableHead>
                    <TableHead>Location</TableHead>
                    <TableHead>Date</TableHead>
                  </TableRow>
                </TableHeader>
                <TableBody>
                  {loading ? (
                    [...Array(5)].map((_, i) => (
                      <TableRow key={i} className="border-neutral-800">
                        <TableCell colSpan={5} className="p-4">
                          <div className="h-6 bg-neutral-800 rounded animate-pulse"></div>
                        </TableCell>
                      </TableRow>
                    ))
                  ) : activityLogs.length === 0 ? (
                    <TableRow>
                      <TableCell colSpan={5} className="text-center py-12 text-neutral-400">
                        No recent activity found.
                      </TableCell>
                    </TableRow>
                  ) : (
                    activityLogs.map((log) => (
                      <TableRow key={log.id} className="border-neutral-800 hover:bg-neutral-800/50">
                        <TableCell className="font-medium capitalize">{log.actionType.replace(/([A-Z])/g, ' $1')}</TableCell>
                        <TableCell className="text-neutral-300">{log.description}</TableCell>
                        <TableCell className="text-neutral-400 text-xs">{log.userAgent}</TableCell>
                        <TableCell className="text-neutral-400 text-xs">{log.location} ({log.ipAddress})</TableCell>
                        <TableCell className="text-neutral-400 text-xs">{format(new Date(log.createdAt), "MMM d, yyyy 'at' h:mm a")}</TableCell>
                      </TableRow>
                    ))
                  )}
                </TableBody>
              </Table>
            </div>
          </CardContent>
        </Card>
      </div>
    </div>
  );
}
